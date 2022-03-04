<template>
  <div class="datepicker-wrapper">
    <input
        v-if="isFormInput"
        :id="name"
        type="hidden"
        :name="name"
        :value="date"
    > 
    <date-picker
        v-model="date"
        value-type="format"
        :format="format"
        :type="type"
        @change="$emit('input', date)"
    />
    <!-- How does this handle date ranges with the hidden input??? -->
  </div>
</template>

<script>
    import DatePicker from 'vue2-datepicker';
    export default {
        name: 'VDatePicker',
        components: {DatePicker},
        props: {
            format: {
                type: String,
                default: 'DD/MM/YYYY'
            },
            type: {
                type: String,
                default: 'date'
            },
            value: {
                type: String
            },
            name: {
                type: String
            },
                isFormInput: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            return {
                date: this.value,
            }
        }
    }
</script>
