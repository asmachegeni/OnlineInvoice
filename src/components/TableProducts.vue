<template>
  <div>
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
      <button type="button" @click="addItem">اضافه کردن ردیف جدید</button>
      <button type="submit">Add Invoice</button>
    </form>
    <button @click="exportExcel()">دانلود فایل اکسل</button>
  </div>
</template>

<script>
import ExcelJS from "exceljs";
import FileSaver from "file-saver";
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
    exportExcel() {
      //  ---------------------------excel--------------------------

      const workbook = new ExcelJS.Workbook();
      const worksheet = workbook.addWorksheet("1");
      worksheet.columns = [
        { header: "شماره فاکتور", key: "invoiceNumber" },
        { header: "Date", key: "date" },
        { header: "Amount", key: "amount" },
      ];
      worksheet.addRow({
        invoiceNumber: "INV-001",
        date: "2023-05-01",
        amount: 100,
      });
      worksheet.addRow({
        invoiceNumber: "INV-002",
        date: "2023-05-02",
        amount: 200,
      });
      workbook.xlsx
        .writeBuffer()
        .then((buffer) =>
          FileSaver.saveAs(new Blob([buffer]), `${Date.now()}_feedback.xlsx`)
        )
        .catch((err) => console.log("Error writing excel export", err));
    },
  },
};
</script>
