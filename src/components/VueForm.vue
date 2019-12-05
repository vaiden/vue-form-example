<template>
    <div class="columns" id="app">
        <div class="column is-two-thirds">
            <section class="section">
                <h1 class="title">Fun with Forms in Vue 2.0</h1>
                <p class="subtitle">
                    Learn how to work with forms, including <strong>validation</strong>!
                </p>
                <hr>
                <!-- form starts here -->
                <section class="form">
                    <form v-on:submit.prevent="$validator.validateAll(); console.log(form);">
                        <div class="field">
                            <label class="label">Name</label>
                            <div class="control">
                                <input name="name" v-model="form.name" v-validate="'required|min:3'" v-bind:class="{'is-danger': errors.has('name')}" class="input" type="text" placeholder="Full name">
                            </div>
                            <p class="help is-danger" v-show="errors.has('name')">
                                {{ errors.first('name') }}
                            </p>
                        </div>

                        <div class="field">
                            <label class="label">Message</label>
                            <div class="control">
                                <textarea name="message" class="textarea" v-validate="'required|polite'" placeholder="Message" v-bind:class="{'is-danger': errors.has('message')}" v-model="form.message"></textarea>
                            </div>
                            <p class="help is-danger" v-show="errors.has('message')">
                                {{ errors.first('message') }}
                            </p>
                        </div>

                        <div class="field">
                            <label class="label">Inquiry Type</label>
                            <div class="control">
                                <div class="select">
                                    <select v-model="form.inquiry_type">
                                        <option disabled value="">Nothing selected</option>
                                        <option v-for="option in options.inquiry" v-bind:value="option.value" v-bind:key="option.key">
                                            {{ option.text }}
                                        </option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <div class="field">
                            <label class="label">LogRocket Usecases</label>
                            <div class="control">
                                <div class="select is-multiple">
                                    <select multiple v-model="form.logrocket_usecases">
                                        <option>Debugging</option>
                                        <option>Fixing Errors</option>
                                        <option>User support</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <div class="field">
                            <div class="control">
                                <label class="checkbox">
                                    <input type="checkbox" v-model="form.terms">
                                    I agree to the <a href="#">terms and conditions</a>
                                </label>
                            </div>
                        </div>

                        <div class="field">
                            <label>
                                <strong>What dev concepts are you interested in?</strong>
                            </label>
                            <div class="control">
                                <label class="checkbox">
                                    <input type="checkbox" v-model="form.concepts"
                                           value="promises">
                                    Promises
                                </label>
                                <label class="checkbox">
                                    <input type="checkbox" v-model="form.concepts"
                                           value="testing">
                                    Testing
                                </label>
                            </div>
                        </div>

                        <div class="field">
                            <label><strong>Is JavaScript awesome?</strong></label>
                            <div class="control">
                                <label class="radio">
                                    <input v-model="form.js_awesome" type="radio" value="Yes">
                                    Yes
                                </label>
                                <label class="radio">
                                    <input v-model="form.js_awesome" type="radio" value="Yeap!">
                                    Yeap!
                                </label>
                            </div>
                        </div>

                        <div class="field is-grouped">
                            <div class="control">
                                <button v-bind:disabled="errors.any()" class="button is-primary">
                                    Submit
                                </button>
                            </div>
                        </div>

                    </form>
                </section>
            </section>
        </div>

        <div class="column">
            <section class="section" id="results">
                <div class="box">
                    <ul>
                        <li v-for="(item, k) in form" v-bind:key="k">
                            <strong>{{ k }}:</strong> {{ item }}</li>
                    </ul>

                </div>
            </section>
        </div>
    </div>
</template>

<script>
    import VeeValidate from 'vee-validate';
    VeeValidate.Validator.extend("polite", {
        getMessage: field => `You need to be polite in the ${field} field`,
        validate: value => value.toLowerCase().indexOf("please") !== -1
    });

    export default {
        name: "VNTForm",
        data: () => ({
            form: {
                name: "",
                message: "",
                inquiry_type: "",
                logrocket_usecases: [],
                terms: false,
                concepts: [],
                js_awesome: ""
            },
            options: {
                inquiry: [
                    { key: 1, value: "feature", text: "Feature Request" },
                    { key: 2, value: "bug", text: "Bug Report" },
                    { key: 3, value: "support", text: "Support" }
                ]
            }
        })
    }
</script>

<style scoped>
    #results {
        position: fixed;
        right: 0;
        top: 10%;
    }
</style>
