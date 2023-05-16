<template>
  <div id="invoice">
    <h1>صورتحساب فروش کالا و خدمات</h1>
    <form @submit.prevent="addInvoice">
      <div class="form-group">
        <label for="Serial_number">شماره سریال</label>
        <input
          type="text"
          id="Serial_number"
          v-model="invoice.Serial_number"
          required
        />
      </div>
      <div class="form-group">
        <label for="invoice_date">تاریخ</label>
        <input
          type="date"
          id="invoice_date"
          v-model="invoice.invoice_date"
          required
        />
      </div>
      <!-- -------------------------------------------------------------- -->

      <div
        class="form-group"
        v-for="(info, index) in invoice.Buyer_and_seller_information"
        :key="index"
      >
        <h2 v-if="info.type === 'seller'">مشخصات فروشنده</h2>
        <h2 v-else>مشخصات خریدار</h2>

        <label for="invoice_date">:نام شخص حقیقی / حقوقی</label>
        <input type="text" id="invoice_date" v-model="info.name" required />
        <label for="invoice_date">:شماره اقتصادی</label>
        <input
          type="text"
          id="invoice_date"
          v-model="info.Economical_number"
          required
        />
        <label for="invoice_date">:شماره ثبت / شماره ملی</label>
        <input
          type="text"
          id="invoice_date"
          v-model="info.registration_number"
          required
        />
        <label for="invoice_date">نشانی کامل: استان:</label>
        <input type="text" id="invoice_date" v-model="info.province" required />
        <label for="invoice_date">شهرستان</label>
        <input type="text" id="invoice_date" v-model="info.county" required />
        <label for="invoice_date">کد پستی 10 رقمی</label>
        <input
          type="text"
          id="invoice_date"
          v-model="info.Postal_code"
          required
        />
        <label for="invoice_date">شهر</label>
        <input type="text" id="invoice_date" v-model="info.city" required />
        <label for="invoice_date">نشانی</label>
        <input type="text" id="invoice_date" v-model="info.address" required />
        <label for="invoice_date">شماره تلفن / نمابر</label>
        <input type="text" id="invoice_date" v-model="info.tel" required />
      </div>

      <!-- ------------------------------------------------------------------------------------- -->
      <div>
        <h2>مشخصات کالا یا خدمات مورد معامله</h2>
        <div>
          <span v-for="i in 11" :key="i">{{ i }}</span>
        </div>
        <div>
          <div>ردیف</div>
          <div>کدکالا</div>
          <div>شرح کالا یا خدمات</div>
          <div>تعداد / مقدار</div>
          <div>واحد اندازه‌گیری</div>
        </div>
        <div>
          <div v-for="(item, index) in invoice.items" :key="index">
            <span> {{ index + 1 }}</span>
            <input type="number" v-model="item.ProductCode" required />
            <input type="text" v-model="item.DescriptionOfGoods" required />
            <input type="number" v-model="item.number" required />
            <input type="number" v-model="item.unit" required />
            <input type="number" v-model="item.UnitAmount" required />
            <input type="number" v-model="item.TotalAmount" required />
            <input type="number" v-model="item.amountDiscount" required />
            <input
              type="number"
              v-model="item.TotalAmountAfterDiscount"
              required
            />
            <input type="number" v-model="item.taxCollection" required />
            <input
              type="number"
              v-model="item.sumOfTotalAmountPlusTax"
              required
            />
            <button type="button" @click="deleteItem(index)">حذف ردیف</button>
          </div>
        </div>
      </div>
      <!-- ------------------------------------------------------------------ -->
      <button type="button" @click="addItem" id="addbtn">
        اضافه کردن ردیف جدید
      </button>
    </form>
    <button @click="exportExcel()" id="excelbtn">دانلود فایل اکسل</button>
    <button @click="generatePDF()" id="pdfbtn">دانلود فایل pdf</button>
    <button @click="print()" id="printbtn">پرینت فاکتور</button>
  </div>
</template>

<script>
import ExcelJS from "exceljs";
import FileSaver from "file-saver";
import jsPDF from "jspdf";
import html2canvas from "html2canvas";
export default {
  data() {
    return {
      invoice: {
        Serial_number: "",
        invoice_date: "",
        Buyer_and_seller_information: [
          {
            type: "seller",
            name: "",
            Economical_number: "",
            registration_number: "",
            province: "",
            county: "",
            Postal_code: "",
            city: "",
            address: "",
            tel: "",
          },
          {
            type: "buyer",
            name: "",
            Economical_number: "",
            registration_number: "",
            province: "",
            county: "",
            Postal_code: "",
            city: "",
            address: "",
            tel: "",
          },
        ],
        items: [
          {
            ProductCode: "",
            DescriptionOfGoods: "",
            number: "",
            unit: "",
            UnitAmount: "",
            TotalAmount: "",
            amountDiscount: "",
            TotalAmountAfterDiscount: "",
            taxCollection: "",
            sumOfTotalAmountPlusTax: "",
          },
        ],
      },
    };
  },
  methods: {
    addItem() {
      this.invoice.items.push({
        ProductCode: "",
        DescriptionOfGoods: "",
        number: "",
        unit: "",
        UnitAmount: "",
        TotalAmount: "",
        amountDiscount: "",
        TotalAmountAfterDiscount: "",
        taxCollection: "",
        sumOfTotalAmountPlusTax: "",
      });
      console.log(this.invoice);
    },
    deleteItem(index) {
      this.invoice.items.splice(index, 1);
    },
    //  ---------------------------excel--------------------------
    exportExcel() {
      const workbook = new ExcelJS.Workbook();
      const worksheet = workbook.addWorksheet("مشخصات کالا");

      worksheet.columns = [
        { header: "کد الا", key: "P" },
        { header: "شرح کالا یا خات", key: "D" },
        { header: "تعدا مقدار", key: "n" },
        { header: "واحد انزه‌گیری", key: "u" },
      ];

      // --------------------------------products---------------------------------------------
      worksheet.getRow(12).values = [
        "کد کالا",
        "شرح کالا یا خدمات",
        "تعداد / مقدار",
        "واحد اندازه‌گیری",
        "مبلغ واحد (ریال) ",
        "مبلغ کل (ریال) ",
        "مبلغ  تخفیف ",
        "مبلغ کل پس از تخفیف ",
        "جمع مالیات و عوارض (ریال) ",
      ];
      worksheet.columns = [
        { header: "کد کالا", key: "ProductCode" },
        { header: "شرح کالا یا خدمات", key: "DescriptionOfGoods" },
        { header: "تعداد / مقدار", key: "number" },
        { header: "واحد اندازه‌گیری", key: "unit" },
        { header: "مبلغ واحد (ریال) ", key: "UnitAmount" },
        { header: "مبلغ کل (ریال) ", key: "TotalAmount" },
        { header: "مبلغ  تخفیف ", key: "amountDiscount" },
        { header: "مبلغ کل پس از تخفیف ", key: "TotalAmountAfterDiscount" },
        { header: "جمع مالیات و عوارض (ریال) ", key: "taxCollection" },
        {
          header: "جمع مبلغ کل بعلاوه جمع مالیات و عوارض (ریال) ",
          key: "sumOfTotalAmountPlusTax",
        },
      ];
      this.invoice.items.forEach((item) => worksheet.addRow(item));

      //---------------------------------seller & buyer information

      worksheet.mergeCells("A1", "W2");
      worksheet.getCell("A1").value = " مشخصات فروشنده";

      worksheet.mergeCells(3, 1, 3, 3);
      worksheet.getCell(3, 1).value = "نام شخص حقیقی / حقوقی";

      worksheet.mergeCells(3, 4, 3, 7);
      worksheet.getCell(3, 4).value =
        this.invoice.Buyer_and_seller_information[0].name;

      worksheet.mergeCells(3, 8, 3, 11);
      worksheet.getCell(3, 8).value = "شماره اقتصادی";
      worksheet.mergeCells(3, 12, 3, 15);
      worksheet.getCell(3, 12).value =
        this.invoice.Buyer_and_seller_information[0].Economical_number;

      worksheet.mergeCells(3, 16, 3, 19);
      worksheet.getCell(3, 16).value = "شماره ثبت / ملی";
      worksheet.mergeCells(3, 20, 3, 23);
      worksheet.getCell(3, 20).value =
        this.invoice.Buyer_and_seller_information[0].registration_number;

      worksheet.mergeCells(4, 1, 4, 3);
      worksheet.getCell(4, 1).value = "نشانی کامل:استان ";

      worksheet.mergeCells(4, 4, 4, 7);
      worksheet.getCell(4, 4).value =
        this.invoice.Buyer_and_seller_information[0].province;

      worksheet.mergeCells(4, 8, 4, 11);
      worksheet.getCell(4, 8).value = "شهرستان ";
      worksheet.mergeCells(4, 12, 4, 15);
      worksheet.getCell(4, 12).value =
        this.invoice.Buyer_and_seller_information[0].county;

      worksheet.mergeCells(4, 16, 4, 19);
      worksheet.getCell(4, 16).value = "کد پستی 10 رقمی ";
      worksheet.mergeCells(4, 20, 4, 23);
      worksheet.getCell(4, 20).value =
        this.invoice.Buyer_and_seller_information[0].Postal_code;

      worksheet.mergeCells(4, 24, 4, 27);
      worksheet.getCell(4, 24).value = "شهر";
      worksheet.mergeCells(4, 28, 4, 31);
      worksheet.getCell(4, 28).value =
        this.invoice.Buyer_and_seller_information[0].city;

      worksheet.mergeCells(5, 1, 5, 3);
      worksheet.getCell(5, 2).value = "نشانی";
      worksheet.mergeCells(5, 4, 5, 7);
      worksheet.getCell(5, 5).value =
        this.invoice.Buyer_and_seller_information[0].address;

      worksheet.mergeCells(5, 8, 5, 11);
      worksheet.getCell(5, 8).value = "شماره تلفن / نمابر";
      worksheet.mergeCells(5, 12, 5, 15);
      worksheet.getCell(5, 13).value =
        this.invoice.Buyer_and_seller_information[0].tel;
      //-------------------------------------------------------------------
      //---------------------------------------------------------------------

      worksheet.mergeCells(6, 1, 7, 23);
      worksheet.getCell(6, 1).value = "مشخصات خریدار";

      worksheet.mergeCells(8, 1, 8, 3);
      worksheet.getCell(8, 1).value = "نام شخص حقیقی / حقوقی";

      worksheet.mergeCells(8, 4, 8, 7);
      worksheet.getCell(8, 4).value =
        this.invoice.Buyer_and_seller_information[1].name;

      worksheet.mergeCells(8, 8, 8, 11);
      worksheet.getCell(8, 8).value = "شماره اقتصادی";
      worksheet.mergeCells(8, 12, 8, 15);
      worksheet.getCell(8, 12).value =
        this.invoice.Buyer_and_seller_information[1].Economical_number;

      worksheet.mergeCells(8, 16, 8, 19);
      worksheet.getCell(8, 16).value = "شماره ثبت / ملی";
      worksheet.mergeCells(8, 20, 8, 23);
      worksheet.getCell(8, 20).value =
        this.invoice.Buyer_and_seller_information[1].registration_number;

      worksheet.mergeCells(9, 1, 9, 3);
      worksheet.getCell(9, 1).value = "نشانی کامل:استان ";

      worksheet.mergeCells(9, 4, 9, 7);
      worksheet.getCell(9, 4).value =
        this.invoice.Buyer_and_seller_information[1].province;

      worksheet.mergeCells(9, 8, 9, 11);
      worksheet.getCell(9, 8).value = "شهرستان ";
      worksheet.mergeCells(9, 12, 9, 15);
      worksheet.getCell(9, 12).value =
        this.invoice.Buyer_and_seller_information[1].county;

      worksheet.mergeCells(9, 16, 9, 19);
      worksheet.getCell(9, 16).value = "کد پستی 10 رقمی ";
      worksheet.mergeCells(9, 20, 9, 23);
      worksheet.getCell(9, 20).value =
        this.invoice.Buyer_and_seller_information[1].Postal_code;

      worksheet.mergeCells(9, 24, 9, 27);
      worksheet.getCell(9, 24).value = "شهر";
      worksheet.mergeCells(9, 28, 9, 31);
      worksheet.getCell(9, 28).value =
        this.invoice.Buyer_and_seller_information[1].city;

      worksheet.mergeCells(10, 1, 10, 3);
      worksheet.getCell(10, 2).value = "نشانی";
      worksheet.mergeCells(10, 4, 10, 7);
      worksheet.getCell(10, 5).value =
        this.invoice.Buyer_and_seller_information[1].address;

      worksheet.mergeCells(10, 8, 10, 11);
      worksheet.getCell(10, 8).value = "شماره تلفن / نمابر";
      worksheet.mergeCells(10, 12, 10, 15);
      worksheet.getCell(10, 13).value =
        this.invoice.Buyer_and_seller_information[1].tel;

      //--------------------------------save excel-----------------------------
      workbook.xlsx
        .writeBuffer()
        .then((buffer) => FileSaver.saveAs(new Blob([buffer]), `فاکتور.xlsx`))
        .catch((err) => console.log("Error writing excel export", err));
    },

    //-------------------------------------pdf---------------------------------------
    generatePDF() {
      const element = document.getElementById("invoice");
      const btns = document.querySelectorAll("button");
      Array.from(btns).forEach((item) => (item.style.opacity = 0));
      element.style.opacity = 1;
      html2canvas(element, { width: 414, height: 736, scale: 2 }).then(
        (canvas) => {
          const imgData = canvas.toDataURL("image/png");
          const pdf = new jsPDF("p", "px");
          pdf.addImage(imgData, "png", 0, 0, 400, 600);
          pdf.save("component.pdf");
        }
      );
    },

    print() {
      window.print();
    },
  },
};
</script>
<style scoped>
#invoice {
  /* width: 400px;
  height: 600px; */
  width: 100%;
  height: 100%;
}

@media print {
  button {
    display: none;
  }
}
</style>
