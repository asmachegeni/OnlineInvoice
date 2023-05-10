<template>
  <div class="header">
    <p v-for="headerName in headerNames" :key="headerName" class="header-name">
      {{ headerName }}
    </p>
  </div>
</template>

<script>
export default {
  name: "TableProducts",
  data() {
    return {
      headerNames: [
        " ",
        "ردیف",
        "کد کالا",
        "َشرح کالا یا خدمات",
        "تعداد / مقدار",
        "واحد اندازه گیری",
        "مبلغ واحد (ریال)",
        "مبلغ کل (ریال)",
        "مبلغ تخفیف",
        "مبلغ کل پس از تخفیف(ریال)",
        "جمع مالیات و عوارض (ریال)",
        "جمع مبلغ کل بعلاوه جمع مالیات و عوارض (ریال)",
      ],
      tableDat: [
        {
          selected: false,
          rownNumber: 1,
          ProductCode: 125,
          DescriptionOfGoods: "کالای خوبی است",
          number: 14,
          unit: "kg",
          UnitAmount: "120000",
          TotalAmount: "6564686979876",
          amountDiscount: "768798",
          TotalAmountAfterDiscount: "87987987",
          taxCollection: 546879879,
          sumOfTotalAmountPlusTax: 467879879879798,
        },
      ],
    };
  },
};
</script>

<style>
.header {
  display: flex;
  flex-direction: row-reverse;
  width: 100%;
  align-items: center;
  justify-content: center;
}
.header-name {
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 36px;
}
</style>
