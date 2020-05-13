<template>
    <div id="employee-form">
        <p v-if="error && submitting" class="error-mesage">
            ❗ 🥰 🔥Please fill out all required fields
        </p>
        <p v-if="success" class="success-mesage">
            ✅ 💘 💣 Employee successfully added
        </p>
        <form v-on:submit.prevent="handleSubmit">
            <label>Employee Name:</label>
            <input
                v-model="employee.name" 
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidName}"
                v-on:focus="clearStatus"
                v-on:keypress="clearStatus"
                ref="first"
            />
            <label>Employee Email:</label>
            <input
                v-model="employee.email"
                type="email"
                v-bind:class="{ 'has-error': submitting && invalidEmail}"
                v-on:focus="clearStatus"
            />
            <button type="submit">Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'employee-form',
    data() {
        return {
            submitting: false,
            error: false,
            success: false,
            employee: {
                name: '',
                email: '',
            }
        }
    },
    methods: {
        handleSubmit() {
            this.submitting = true,
            this.clearStatus()

            if (this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }
            this.$emit('add:employee', this.employee)
            this.$refs.first.focus()
            this.employee = {
                name: '',
                email: ''
            }
            this.error = false
            this.success = true
            this.submitting = false
        },
        clearStatus() {
            this.error = false,
            this.success = false
        }
    },
    computed: {
        invalidName() {
            return this.employee.name ===''
        },
        invalidEmail() {
            return this.employee.email === ''
        }
    }
}
</script>

<style>
    form {
        margin-bottom: 2rem;
    }
    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>