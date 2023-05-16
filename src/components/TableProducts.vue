<template>
  <div id="invoice">
    <!-- <form @submit.prevent="addInvoice"> -->
    <div id="date-serial">
      <div class="form-group">
        <label for="Serial_number" id="lab1">شماره سریال</label>
        <input
          type="text"
          id="Serial_number"
          v-model="invoice.Serial_number"
          required
          placeholder="......................"
        />
      </div>
      <div class="form-group">
        <label for="invoice_date" id="lab2">تاریخ</label>
        <input
          type="date"
          class="invoice_date"
          v-model="invoice.invoice_date"
          required
        />
      </div>
    </div>
    <h1 id="title">صورتحساب فروش کالا و خدمات</h1>
    <!-- -------------------------------------------------------------- -->

    <div
      v-for="(info, index) in invoice.Buyer_and_seller_information"
      :key="index"
    >
      <h2 v-if="info.type === 'seller'">مشخصات فروشنده</h2>
      <h2 v-else>مشخصات خریدار</h2>

      <div class="form-group2">
        <div class="section">
          <label for="invoice_date">نام شخص حقیقی / حقوقی</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.name"
            required
            placeholder="......................"
          />
          <label for="invoice_date">شماره اقتصادی</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.Economical_number"
            required
            placeholder="......................"
          />
          <label for="invoice_date">شماره ثبت / شماره ملی</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.registration_number"
            required
            placeholder="......................"
          />
        </div>
        <div class="section">
          <label for="invoice_date">نشانی کامل: استان</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.province"
            required
            placeholder="......................"
          />
          <label for="invoice_date">شهرستان</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.county"
            required
            placeholder="......................"
          />
          <label for="invoice_date">کد پستی 10 رقمی</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.Postal_code"
            required
            placeholder="......................"
          />
          <label for="invoice_date">شهر</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.city"
            required
            placeholder="......................"
          />
        </div>
        <div class="section">
          <label for="invoice_date">نشانی</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.address"
            required
            placeholder="......................"
          />
          <label for="invoice_date">شماره تلفن / نمابر</label>
          <input
            type="text"
            class="invoice_date"
            v-model="info.tel"
            required
            placeholder="......................"
          />
        </div>
      </div>
    </div>

    <!-- ------------------------------------------------------------------------------------- -->
    <div>
      <h2>مشخصات کالا یا خدمات مورد معامله</h2>

      <div class="columns">
        <div class="headers">
          <div class="title-header-number" v-for="i in 11" :key="i">
            {{ i }}
          </div>
        </div>
        <div class="headers">
          <div class="title-header">ردیف</div>
          <div class="title-header">کدکالا</div>
          <div class="title-header">شرح کالا یا خدمات</div>
          <div class="title-header">تعداد / مقدار</div>
          <div class="title-header">واحد اندازه‌گیری</div>
          <div class="title-header">مبلغ واحد (ریال)</div>
          <div class="title-header">مبلغ کل (ریال)</div>
          <div class="title-header">مبلغ تخفیف</div>
          <div class="title-header">مبلغ کل پس از تخفیف</div>
          <div class="title-header">جمع مالیات و عوارض (ریال)</div>
          <div class="title-header">
            جمع مبلغ کل بعلاوه جمع مالیات و عوارض (ریال)
          </div>
        </div>
        <div v-for="(item, index) in invoice.items" :key="index" class="col">
          <div class="row-invoice">{{ index + 1 }}</div>
          <input
            class="col-invoice"
            type="number"
            v-model="item.ProductCode"
            required
          />
          <input
            class="col-invoice"
            type="text"
            v-model="item.DescriptionOfGoods"
            required
          />
          <input
            class="col-invoice"
            type="number"
            v-model="item.number"
            required
          />
          <input class="col-invoice" type="text" v-model="item.unit" required />
          <input
            class="col-invoice"
            type="number"
            v-model="item.UnitAmount"
            required
          />
          <input
            class="col-invoice"
            type="number"
            v-model="item.TotalAmount"
            required
          />
          <input
            class="col-invoice"
            type="number"
            v-model="item.amountDiscount"
            required
          />
          <input
            type="number"
            v-model="item.TotalAmountAfterDiscount"
            required
            class="col-invoice"
          />
          <input
            type="number"
            class="col-invoice"
            v-model="item.taxCollection"
            required
          />
          <input
            type="number"
            v-model="item.sumOfTotalAmountPlusTax"
            required
            class="col-invoice"
          />
          <button type="button" @click="deleteItem(index)" id="deletebtn">
            حذف
          </button>
        </div>
        <div class="col">
          <div class="total">جمع کل</div>

          <div class="totalCol">
            {{ totalTotalAmount() }}
          </div>
          <div class="totalCol">{{ totalamountDiscount() }}</div>
          <div class="totalCol">{{ totalTotalAmountAfterDiscount() }}</div>
          <div class="totalCol">{{ totaltaxCollection() }}</div>
          <div class="totalCol">{{ totalsumOfTotalAmountPlusTax() }}</div>
        </div>
        <div class="col">
          <div class="desc1">
            <p>شرایط و نحوه فروش</p>
            <p>نقدی</p>
            <input type="checkbox" />
            <p>غیر نقدی</p>
            <input type="checkbox" />
          </div>
        </div>
        <div class="col">
          <div class="desc">
            <p>توضیحات</p>
            <textarea placeholder="..................."></textarea>
          </div>
        </div>
        <div class="col">
          <div class="col2">مهر و امضا فروشنده</div>
          <div class="col3">مهر وامضا خریدار</div>
        </div>
      </div>
    </div>
    <!-- ------------------------------------------------------------------ -->

    <!-- </form> -->
    <div class="btns">
      <button type="button" @click="addItem" id="addbtn">
        اضافه کردن ردیف جدید
      </button>
      <button @click="exportExcel()" id="excelbtn">دانلود فایل اکسل</button>
      <button @click="generatePDF()" id="pdfbtn">دانلود فایل pdf</button>
      <button @click="print()" id="printbtn">پرینت فاکتور</button>
    </div>
  </div>
</template>

<script>
import ExcelJS from "exceljs";
import FileSaver from "file-saver";
import html2pdf from "html2pdf.js";
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
    },
    deleteItem(index) {
      this.invoice.items.splice(index, 1);
    },
    totalTotalAmount() {
      let sum = 0;
      this.invoice.items.forEach((item) => (sum += item.TotalAmount));
      return sum;
    },
    totalamountDiscount() {
      let sum = 0;
      this.invoice.items.forEach((item) => (sum += item.amountDiscount));
      return sum;
    },
    totalTotalAmountAfterDiscount() {
      let sum = 0;
      this.invoice.items.forEach(
        (item) => (sum += item.TotalAmountAfterDiscount)
      );
      return sum;
    },
    totaltaxCollection() {
      let sum = 0;
      this.invoice.items.forEach((item) => (sum += item.taxCollection));
      return sum;
    },
    totalsumOfTotalAmountPlusTax() {
      let sum = 0;
      this.invoice.items.forEach(
        (item) => (sum += item.sumOfTotalAmountPlusTax)
      );
      return sum;
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
      worksheet.mergeCells(12, 1, 13, 23);
      worksheet.getCell(12, 1).value = " مشخصات کالا یا خدمات مورد معامله";
      worksheet.getRow(14).values = [
        "کد کالا",
        "شرح کالا یا خدمات",
        "تعداد / مقدار",
        "واحد اندازه‌گیری",
        "مبلغ واحد (ریال) ",
        "مبلغ کل (ریال) ",
        "مبلغ  تخفیف ",
        "مبلغ کل پس از تخفیف ",
        "جمع مالیات و عوارض (ریال) ",
        "جمع مبلغ کل بعلاوه جمع مالیات و عوارض (ریال)",
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
        {
          header: "جمع مبلغ کل بعلاوه جمع مالیات و عوارض (ریال)",
          key: "taxCollection",
        },
        {
          header: "جمع مالیات و عوارض (ریال) ",
          key: "sumOfTotalAmountPlusTax",
        },
      ];
      this.invoice.items.forEach((item) => worksheet.addRow(item));
      let row = 15 + this.invoice.items.length;
      worksheet.mergeCells(row, 1, row, 5);
      worksheet.getCell(row, 1).value = "جمع کل";
      worksheet.getCell(row, 6).value = this.totalTotalAmount();
      worksheet.getCell(row, 7).value = this.totalamountDiscount();
      worksheet.getCell(row, 8).value = this.totalTotalAmountAfterDiscount();
      worksheet.getCell(row, 9).value = this.totaltaxCollection();
      worksheet.getCell(row, 10).value = this.totalsumOfTotalAmountPlusTax();

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

      var opt = {
        margin: 0,
        filename: "فاکتور.pdf",
        image: { type: "PNG", quality: 1 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: "mm", format: "A4", orientation: "landscape" },
      };

      html2pdf().from(element).set(opt).save();
      setTimeout(() => {
        Array.from(btns).forEach((item) => (item.style.opacity = 1));
      }, 10);
    },

    print() {
      const btns = document.querySelectorAll("button");
      Array.from(btns).forEach((item) => (item.style.opacity = 0));

      window.print();
      setTimeout(() => {
        Array.from(btns).forEach((item) => (item.style.opacity = 1));
      }, 10);
    },
  },
};
</script>
<style>
@font-face {
  font-family: "Vazirmatn Bold";
  src: url("./../assets/fonts/Vazirmatn-Bold.ttf"),
    url("./../assets/fonts/Vazirmatn-Bold.woff2");
}
@font-face {
  font-family: "Vazirmatn Medium";
  src: url("./../assets/fonts/Vazirmatn-Medium.ttf"),
    url("./../assets/fonts/Vazirmatn-Medium.woff2");
}
@font-face {
  font-family: "Vazirmatn Regular";
  src: url("./../assets/fonts/Vazirmatn-Regular.ttf"),
    url("./../assets/fonts/Vazirmatn-Regular.woff2");
}
#invoice {
  direction: rtl;
  width: 90%;
  height: 100%;
  display: flex;
  flex-direction: column;
  font-family: "Vazirmatn Regular";
  font-size: 14px;
  color: #000;
  overflow-x: hidden;
  justify-content: center;
  align-items: center;
  margin: auto;
  background-color: #fff;
}
.desc1 {
  border: 1px solid #2d2d2d;
  height: 50px;
  width: 400px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.col2,
.col3 {
  display: flex;
  border: 1px solid #2d2d2d;
  width: 400px;
  height: 80px;
}
.desc {
  border: 1px solid #2d2d2d;
  height: 35px;
  width: 400px;
  display: flex;
  /* flex-direction: column; */
  align-items: flex-start;
  justify-content: space-between;
}
#title {
  font-family: "Vazirmatn Bold";
  font-size: 22px;
  color: #000;
  text-align: center;
  margin-top: 50px;
}
h2 {
  font-family: "Vazirmatn Bold";
  font-size: 18px;
  color: #000;
  text-align: center;
}
textarea {
  border: none;
  resize: none;
  outline: none;
}
#header {
  display: flex;
}
.form-group2 {
  border: 1px solid #2d2d2d;
  height: 110px;
  width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.headers {
  display: flex;
  margin-left: 32px;
}
.title-header:nth-child(1) {
  display: block;
  height: 54px;
  width: 8px;
  font-size: 8px;
  text-orientation: mixed;
  /* margin-bottom: 38px; */
}
.title-header-number:nth-child(1) {
  display: block;
  height: 20px;
  width: 8px;
  /* margin-bottom: 38px; */
}
.title-header-number {
  border: 1px solid #2d2d2d;
  height: 20px;
  width: 94px;
  display: block;
  font-size: 12px;
  text-align: center;
  display: block;
}
.title-header {
  height: 54px;
  width: 94px;
  display: block;
  font-size: 12px;
  text-align: center;
  display: block;
  border: 1px solid #2d2d2d;
}
p {
  margin: 4px;
}
#date-serial {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  position: absolute;

  left: 6%;
  top: 2%;
}
input {
  font-family: "Vazirmatn Regular";
  font-size: 12px;
}
.form-group {
  display: flex;
  margin-bottom: 4px;
  align-items: center;
  justify-content: center;
}
.totalCol {
  border: 1px solid #2d2d2d;
  height: 28px;
  width: 94px;
  display: block;
}
#lab1,
#lab2 {
  display: block;
  width: 80px;
  text-align: right;
}
label {
  display: block;
  width: 140px;
  font-size: 12px;
}
.section {
  display: flex;
  align-items: center;
  justify-content: start;
  width: 100%;
  text-align: left;
  margin-bottom: 8px;
  height: 24px;
}
.col-invoice {
  border: 1px solid #2d2d2d;
  height: 28px;
  width: 90px;
  display: block;
}
.total {
  width: 486px;
  height: 28px;
  border: 1px solid #2d2d2d;
}
.col {
  display: flex;
  align-items: center;
  align-self: flex-start;
  margin: auto;
  margin-bottom: 1px;
  width: 90%;
  margin-top: 0;
}
.row-invoice {
  display: block;
  border: 1px solid #2d2d2d;
  height: 30px;
  width: 28px;
}
.columns {
  display: flex;
  align-items: center;
  justify-content: start;
  flex-direction: column;
}
#Serial_number,
.invoice_date {
  margin-right: 4px !important;
  border-radius: 4px !important;
  border: none;
  height: 28px;
  width: 80px !important;
  outline: none;
}
#deletebtn {
  width: 120px;
  height: 31px;
  color: #fff;
  font-family: "Vazirmatn Regular";
  font-size: 12px;
  background-color: red;
  display: block;
  border: none;
  margin-right: 2px;

  border-radius: 4px;
  cursor: pointer;
}
#addbtn {
  width: 140px;
  height: 31px;
  color: #fff;
  font-family: "Vazirmatn Regular";
  font-size: 14px;
  background-color: goldenrod;
  display: block;
  border: none;
  margin-right: 2px;
  border-radius: 4px;
  cursor: pointer;
}
#excelbtn {
  width: 140px;
  height: 31px;
  color: #fff;
  font-family: "Vazirmatn Regular";
  font-size: 14px;
  background-color: green;
  display: block;
  border: none;
  margin-right: 2px;
  border-radius: 4px;
  cursor: pointer;
}
#pdfbtn {
  width: 140px;
  height: 31px;
  color: #fff;
  font-family: "Vazirmatn Regular";
  font-size: 14px;
  background-color: brown;
  display: block;
  border: none;
  margin-right: 2px;
  border-radius: 4px;
  cursor: pointer;
}
#printbtn {
  width: 140px;
  height: 31px;
  color: #fff;
  font-family: "Vazirmatn Regular";
  font-size: 14px;
  background-color: purple;
  display: block;
  border: none;
  margin-right: 2px;
  border-radius: 4px;
  cursor: pointer;
}
.btns {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;
  margin-top: 40px;
  margin-bottom: 40px;
}
@media screen and (max-width: 1440px) {
  label {
    width: 80px;
    font-size: 10px;
  }
  .invoice_date {
    width: 80px !important;
  }
  #invoice {
    overflow-x: auto;
    width: 100%;
  }
  body {
    overflow-x: auto;
  }
}
@media screen and (max-width: 992px) {
  #invoice {
    overflow-x: auto;
    width: 150%;
  }
}
@media screen and (max-width: 768px) {
  #invoice {
    overflow-x: auto;
    width: 220%;
  }
}
@media print {
  button {
    display: none;
  }
}
</style>
