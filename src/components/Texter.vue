<template>
    <div id="texter" class="popup">
        <div id="texter-content" class="popup-content" role="dialog">
            <header id="texter-header">
                <slot name="header">
                    <b-navbar toggleable="lg" type="dark" variant="dark">
                        <b-navbar-brand>
                            {{ wrapper.title }}
                        </b-navbar-brand>
                        <b-navbar-nav class="ml-auto">
                            <b-nav-form>
                                <b-icon class="h3 m-1" icon="x-circle-fill" @click="close(true)" variant="danger" />
                            </b-nav-form>
                        </b-navbar-nav>
                    </b-navbar>
                </slot>
            </header>
            <section id="texter-body">
                <slot name="body">
                    <b-card>
                        {{ wrapper.identifier }} *
                        <b-form-input size="sm" type="text" v-model="value" @keyup.enter="value.length != 0 ? close(false) : undefined" />
                    </b-card>
                </slot>
            </section>
            <footer id="texter-footer" class="centered">
                <slot name="footer">
                    <b-card>
                        <fragment v-if="wrapper.value === undefined">
                            <b-button class="m-1" type="button" variant="dark" @click="close(false)" :disabled='value.length == 0'>
                                Confirm
                            </b-button>
                        </fragment>
                        <fragment v-else>
                            <b-button class="m-1" type="button" variant="dark" @click="close(false)" :disabled='value.length == 0'>
                                Save
                            </b-button>
                            <b-button class="m-1" type="button" variant="dark" @click="close(true)">
                                Discard
                            </b-button>
                        </fragment>
                    </b-card>
                </slot>
            </footer>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Texter',

        data() {
            return {
                /** @type {!String} */
                value: ''
            }
        },
        mounted() {
            if (typeof this.wrapper.value === 'string') {
                this.value = this.wrapper.value;
            }
        },
        props: {
            'wrapper': { callback: !Function, title: !String, identifier: !String, value: !String|undefined }
        },

        methods: {
            /**
             * @param {!Boolean} canceled
             */
            close(canceled) {
                if (!canceled) {
                    this.wrapper.callback(this.value);
                }
                this.$emit('close');
            }
        }
    };
</script>
