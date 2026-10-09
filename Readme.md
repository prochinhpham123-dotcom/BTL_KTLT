std::string unit = (type == CDRType::VOICE) ? " giây" : (type == CDRType::SMS ? " bản" : " MB");

        std::cout << "[Bản ghi CDR] Mã: " << cdrId 
                  << " | SĐT: " << phoneNumber 
                  << " | Loại: " << typeStr 
                  << " | Dung lượng: " << durationOrData << unit 
                  << " | Thời gian: " << timestamp << "\n";
    }
};


int main() {
   
    Subscriber sub("0987654321", 5000.0);
    std::cout << "=== QUẢN LÝ THUÊ BÀO DI ĐỘNG ===\n";
    sub.displayInfo();

    
    std::cout << "\n--- 1. XỬ LÝ PHIẾU NẠP TIỀN ---\n";
    TopupReceipt receipt1("TX1001", sub.getPhoneNumber(), 50000.0, TopupMethod::CARD);
    if (receipt1.processTopup(sub, "CARD_CODE_12345")) {
        receipt1.printReceipt();
    }
    sub.displayInfo();

   
    std::cout << "\n--- 2. TIẾP NHẬN & GIẢI MÃ BẢN GHI CDR ---\n";
    
   
    std::string rawCDR1 = "CDR2001|0987654321|VOICE|150|2026-10-09 09:15:00";
    std::string rawCDR2 = "CDR2002|0987654321|DATA|200|2026-10-09 09:30:00";

   
    auto cdr1 = CDRRecord::parseRawCDR(rawCDR1);
    if (cdr1) {
        cdr1->displayCDR();
        cdr1->processCDR(sub);
    }

  
    auto cdr2 = CDRRecord::parseRawCDR(rawCDR2);
    if (cdr2) {
        cdr2->displayCDR();
        cdr2->processCDR(sub);
    }

    std::cout << "\n--- THÔNG TIN THUÊ BÀO SAU CÁC GIAO DỊCH ---\n";
    sub.displayInfo();

    return 0;
}
