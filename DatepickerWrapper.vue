<template>
  <div class="datepicker-wrapper test">
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
                format: 'DD/MM/YYYY',
                type: 'date',
                date: this.value,
            }
        },
        created() { // convert the iso date range array values prop to the format required
            if (this.date && this.isRange) {
                const startDate = this.splitDate(this.date[0], '-')
                this.date[0] = `${startDate[2]}/${startDate[1]}/${startDate[0]}`
                const endDate = this.splitDate(this.date[1], '-')
                this.date[1] = `${endDate[2]}/${endDate[1]}/${endDate[0]}`
            }
        },
        methods: {
            splitDate(value, character = '/') {
                value = value.includes('T') ? value.split('T')[0] : value
                value = value.includes(' ') ? value.split(' ')[0] : value
                return value.split(character)
            },
            isoToDate(value) { //value needs to be yyyy-mm-dd to be converted to a Date
                const datePieces = this.splitDate(value)
                return `${datePieces[2]}-${datePieces[1]}-${datePieces[0]}`
            },
            dateToIso(date) { // convert the value to iso to save in the backend
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
                    if (this.isRange) { //need to convert formattedDate from Y-m-d to d/m/Y
                        const dateStart = new Date(this.isoToDate(this.date[0]))
                        const dateEnd =  new Date(this.isoToDate(this.date[1]))
                        return [this.dateToIso(dateStart), this.dateToIso(dateEnd)]
                    }
                    return this.dateToIso(new Date(this.isoToDate(this.date)))
                }
                else {
                    return null
                }
            },
        },
    }
</script>
<!-- Ensure that the preset table styles are not affecting the date calendar at large screens down -->