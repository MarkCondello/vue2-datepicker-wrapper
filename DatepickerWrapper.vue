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
              unconvertedDates: false,
                counter: 0
            }
        },
        methods: {
            dateToIso(date) {
               this.counter > 0 ? console.log('reached datetoiso', date) : null
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
                
                    //     let start = this.date[0]
                    //     let end = this.date[1]
                    //    this.counter > 0 ? console.log('date', this.date, {start, end}) : null;

                            //format needs to be yyyy-mm-dd to be converted to a Date
                        let dateStart =  this.unconvertedDates ? Date(this.date[0]) : this.date[0]
                        let dateEnd =  this.unconvertedDates ? new Date(this.date[1]) : this.date[1]

                         this.counter > 0 ? console.log({dateStart, dateEnd}) : null;
                         this.counter > 0 ? console.log(dateStart instanceof Date, dateEnd instanceof Date) : null;

                        let startFormat = this.dateToIso(dateStart)
                        let endFormat = this.dateToIso(dateEnd)

                        this.counter > 0 ? console.log({startFormat, endFormat}) : null;
                        this.unconvertedDates = false
                        this.counter++
                        console.log('log in formattedDate')
                        return [startFormat, endFormat]
                       
                    }
                    let date = new Date(this.date)
                    return this.dateToIso(date)
                }
                else {
                    return null
                }
            },
        },
    }
</script>
<!-- Ensure that the preset table styles are not affecting the date calendar at large screens down -->