<template>
    <div class="collapse" ref="collapse">
        <slot></slot>
    </div>
</template>

<script>
    export default {
        props: {
            visible: {
                default: false,
                type: Boolean,
                required: true
            }
        },

        data: function() {
            return {
                collapse: null,
                state: this.visible ? 'shown' : 'hidden'
            }
        },

        mounted: function() {
            this.collapse = $(this.$refs.collapse);

            if( this.visible ) {
                this.show();
            } else {
                this.hide();
            }

            var vm = this;
            this.collapse
                    .on('show.bs.collapse', function (e) {
                        if( $(this).is(e.target) ) {
                            vm.state = 'showing';
                            vm.$emit('collapse-show', e);
                        }
                    })
                    .on('shown.bs.collapse', function (e) {
                        if( $(this).is(e.target) ) {
                            vm.state = 'shown';
                            vm.$emit('collapse-shown', e);
                        }
                    })
                    .on('hide.bs.collapse', function (e) {
                        if( $(this).is(e.target) ) {
                            vm.state = 'hiding';
                            vm.$emit('collapse-hide', e);
                        }
                    })
                    .on('hidden.bs.collapse', function (e) {
                        if( $(this).is(e.target) ) {
                            vm.state = 'hidden';
                            vm.$emit('collapse-hidden', e);
                        }
                    });
        },

        watch: {
            visible: function(val, oldVal) {
                if( val === oldVal ) return;
                // we need to perform some event handling and handler clearing in order to prevent jquery from falling out of sync with vue.
                if( val === true ) {
                    if( this.state == 'hidden' ) {
                        this.show();
                    } else if( this.state == 'hiding' ) {
                        this.$once('collapse-hidden', this.show );
                    } else {
                        // we are already in (or transitioning to) the expected state,
                        // so remove any additional event handlers that could modify this further
                        this.$off('collapse-shown');
                    }

                } else {
                    if( this.state == 'shown' ) {
                        this.hide();
                    } else if( this.state == 'showing' ) {
                        this.$once('collapse-shown', this.hide );
                    } else {
                        // we are already in (or transitioning to) the expected state,
                        // so remove any additional event handlers that could modify this further
                        this.$off('collapse-hidden');
                    }
                }
            }
        },

        methods: {
            show: function() {
                var vm = this;
                // added nextTick because it gives vue the opportunity to render any dom changes before executing the collapse
                // this allows the collapse the expand to the proper size
                // without this the animation can appear broken or choppy if we are changing elements inside the collapse with v-if
                Vue.nextTick(function(){
                    vm.collapse.collapse('show');
                });
            },
            hide: function() {
                // here we do not have a nextTick call because we DONT want vue to render any changes inside the collapse for the same reasons as above.
                // if we remove elements with v-if BEFORE calling collapse(hide) then it will appear choppy and broken
                this.collapse.collapse('hide');
            }
        }
    }
</script>
