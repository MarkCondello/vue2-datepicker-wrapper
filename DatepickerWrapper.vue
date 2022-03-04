<template>
  <div class="datepicker-wrapper">
    <input
        v-if="isFormInput"
        :id="name"
        type="hidden"
        :name="name"
        :value="formattedDate"
    > 
    <date-picker
        v-model="date"
        value-type="format"
        :disabled="isDisabled"
        :format="format"
        :range="isRange"
        :type="type"
        @change="$emit('input', date)"
    />
  </div>
</template>

<script>
    import DatePicker from 'vue2-datepicker';
    export default {
        name: 'datepickerWrapper',
        components: {DatePicker},
        props: {
            format: {
                type: String,
                default: 'DD/MM/YYYY',
            },
            type: {
                type: String,
                default: 'date',
            },
            value: {
                type: String | Array,
            },
            name: {
                type: String,
            },
            isFormInput: {
                type: Boolean,
                default: false,
            },
            isRange: {
                type: Boolean,
                default: false,
            },
            isDisabled: {
                default: false,
                type: Boolean,
            }
        },
        data() {
            return {
                date: this.value,
            }
        },
        methods: {
            dateToIso(date) {
                return (date instanceof Date)
                    ? `${date.getFullYear()}-${this.padZero(date.getMonth() + 1)}-${this.padZero(date.getDate())}`
                    : null
            },
            padZero(n) {
                return n < 10 ? `0${n}` : n
            },
        },
        computed: {
            formattedDate() {
                if (this.date) {
                    if (this.isRange) {
                        let dateStart = new Date(this.date[0])
                        let dateEnd = new Date(this.date[1])
                        return [this.dateToIso(dateStart), this.dateToIso(dateEnd)]
                    } else {
                        let date = new Date(this.date)
                        return this.dateToIso(date)
                    }
                }
                else {
                    return null
                }
            },
        },
    }
</script>
<!-- Ensure that the preset table styles are not affecting the date calendar at large screens down -->