<template>
  <ElCard class="form-card">
    <ElForm :model="formData" ref="addItemForm" :rules="rules" label-position="top">
      <ElFormItem label="Type" prop="type">
        <ElSelect class="type-select" v-model="formData.type" placeholder="Choose type...">
          <ElOption label="Income" value="INCOME"/>
          <ElOption label="Outlay" value="OUTLAY"/>
        </ElSelect>
      </ElFormItem>
      <ElFormItem label="Comments" prop="comment">
        <ElInput v-model="formData.comment"/>
      </ElFormItem>
      <ElFormItem label="Value" prop="value">
        <ElInput v-model.number="formData.value"/>
      </ElFormItem>
      <ElButton @click="onSubmit" type="primary">Submit</ElButton>
    </ElForm>
  </ElCard>
</template>

<script>
export default {
  name: "Form",
  data() {
    return {
      formData: {
        type: 'INCOME',
        comment: '',
        value: 0
      },
      rules: {
        type: [{required: true, message: 'Please select type', trigger: 'blur'}],
        comments: [{required: true, message: 'Please leave some comment', trigger: 'change'}],
        value: [{required: true, message: 'Please write a  valid value', trigger: 'change'},
          {type: "number", message: 'Value must be a number', trigger: 'change'}],
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.addItemForm.validate(valid => {
        if (valid) {
          const form = this.formData

          if (form.type === 'INCOME') {
            form.value = Math.abs(form.value)
          }
          else if(form.type === 'OUTLAY') {
            form.value = -Math.abs(form.value)
          }

          this.$emit('submitForm', {...this.formData});
          this.$refs.addItemForm.resetFields();
        }
      });
    }
  }
}
</script>

<style scoped>
.form-card {
  max-width: 500px;
  margin: auto;
}

.type-select {
  width: 100%;
}
</style>