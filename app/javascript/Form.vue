<template>
  <div class="form-wrapper">
    <h1 class="form__header">{{ $t('form.invoiceForm.header') }}</h1>
    <Form :model="resource" ref="form">
      <FormGroup
        prop="invoiceKind"
        :label="$t('form.invoiceForm.fields.invoiceKind.label')"
        :help="$t('form.invoiceForm.fields.invoiceKind.inputHelp')"
        :adaptivePlaceholder="true"
      >
        <Select
          v-model="resource.invoiceKind"
          :placeholder="$t('form.invoiceForm.fields.invoiceKind.placeholder')"
          name="invoiceKind"
        >
          <Option
            v-for="option in invoiceKinds"
            :key="option.value"
            :label="option.name"
            :value="option.value"
          />
        </Select>
      </FormGroup>

      <FormGroup
        prop="number"
        :label="$t('form.invoiceForm.fields.number.label')"
        :adaptivePlaceholder="true"
      >
        <TextInput
          name="number"
          :placeholder="$t('form.invoiceForm.fields.number.label')"
          type="text"
          v-model="resource.number"
          required
        >
        </TextInput>
      </FormGroup>

      <FormGroup
        prop="ks"
        :label="$t('form.invoiceForm.fields.ks.label')"
        :adaptivePlaceholder="true"
        :optional="true"
        :halfSize="true"
      >
        <TextInput
          name="ks"
          type="text"
          :placeholder="$t('form.invoiceForm.fields.ks.label')"
          v-model="resource.ks"
        >
        </TextInput>
      </FormGroup>

      <FormGroup
        prop="vs"
        :label="$t('form.invoiceForm.fields.vs.label')"
        :adaptivePlaceholder="true"
        :optional="true"
        :halfSize="true"
      >
        <TextInput
          name="vs"
          type="text"
          :placeholder="$t('form.invoiceForm.fields.vs.label')"
          v-model="resource.vs"
        >
        </TextInput>
      </FormGroup>

      <FormGroup
        prop="issedOn"
        :label="$t('form.invoiceForm.fields.issedOn.label')"
        :adaptivePlaceholder="true"
      >
        <DatePicker
          v-model="resource.issedOn"
          value-format="timestamp"
          :clearable="false"
          :placeholder="$t('form.invoiceForm.fields.issedOn.placeholder')"
        />
      </FormGroup>

      <FormGroup
        prop="maturityKind"
        :label="$t('form.invoiceForm.fields.maturityKind.label')"
        :adaptivePlaceholder="true"
        :halfSize="true"
      >
        <Select
          v-model="resource.maturityKind"
          :placeholder="$t('form.invoiceForm.fields.maturityKind.placeholder')"
          name="maturityKind"
        >
          <Option
            v-for="option in maturityKinds"
            :key="option.value"
            :label="option.name"
            :value="option.value"
          />
        </Select>
      </FormGroup>

      <div class="el-form-item el-form-item--half-size deadline">
        {{ deadline }}
      </div>
    </Form>
  </div>
</template>

<script>
import { addDays, format } from 'date-fns'

export default {
  name: 'BasicForm',
  data: () => ({
    resource: {
      number: '',
      vs: '',
      ks: '',
      issedOn: '',
      invoiceKind: 'vat_invoice'
    },
    invoiceKinds: [
      { value: 'vat_invoice', name: 'Faktura s DPH' },
      { value: 'invoice', name: 'Faktura bez DPH' },
      { value: 'proforma', name: 'Proforma' }
    ],
    maturityKinds: [
      { value: '7', name: '7 dnů' },
      { value: '14', name: '14 dnů' },
      { value: '21', name: '21 dnů' }
    ],
    deadline: null
  }),
  watch: {
    'resource.maturityKind' (value) {
      if (value) {
        this.deadline = this.calculateDeadline(value)
      }
    }
  },
  methods: {
    calculateDeadline (days) {
      const today = new Date()
      const deadline = addDays(today, parseInt(days))
      return format(deadline, 'd. M. yyyy')
    }
  }
}
</script>
