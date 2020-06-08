<template>
    <div :class="elementSize">
        <field-wrapper :stacked="field.stacked">
            <div class="px-10" :class="field.stacked ? 'pt-4 w-full' : 'py-6 w-1/5'">
                <slot class="">
                    <form-label
                            :label-for="field.attribute"
                            :class=""
                    >
                        {{ fieldLabel }}

                        <span v-if="field.required" class="text-danger text-sm">{{
            __('*')
          }}</span>
         <!--  <span v-if="!field.required" class="text-sm">{{
            __('(Optional)')
          }}</span> -->
            <span v-if="field.helpText" style="color: text-80" v-tooltip="field.helpText"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" height="15px" width="15px" role="img" aria-hidden="true"><path d="M15.07,11.25L14.17,12.17C13.45,12.89 13,13.5 13,15H11V14.5C11,13.39 11.45,12.39 12.17,11.67L13.41,10.41C13.78,10.05 14,9.55 14,9C14,7.89 13.1,7 12,7A2,2 0 0,0 10,9H8A4,4 0 0,1 12,5A4,4 0 0,1 16,9C16,9.88 15.64,10.67 15.07,11.25M13,19H11V17H13M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12C22,6.47 17.5,2 12,2Z"></path></svg></span>
                    </form-label>
                </slot>
            </div>
            <div class="py-6 px-8" :class="fieldClasses">
                <slot name="field"/>

                <help-text
                  class="error-text mt-2 text-danger"
                  v-if="showErrors && hasError"
                >
                  {{ firstError }}
                </help-text>


            </div>
        </field-wrapper>
    </div>
</template>

<script>
    import {HandlesValidationErrors, Errors} from 'laravel-nova'
    import VTooltip from 'v-tooltip'

    export default {
        mixins: [HandlesValidationErrors],

        props: {
            field: {type: Object, required: true},
            fieldName: {type: String},
            showHelpText: {type: Boolean, default: false},
            showErrors: {type: Boolean, default: true},
            fullWidthContent: {type: Boolean, default: false},
        },

        components: {
            VTooltip,
        },

        mounted() {
            // If field has a size, this allows to use flex on card
            if(this.hasSize) {
                this.$parent.$parent.$el.classList.add('flex-wrap')
                this.$parent.$parent.$el.classList.add('flex')
            }

            if(this.getRemoveBottomBorder === true){
                this.$el.children[0].classList.add('remove-bottom-border')
            } else if(this.getRemoveBottomBorder === false){
                this.$el.children[0].classList.remove('remove-bottom-border')
            }
        },

        computed: {
            /**
             * Return the label that should be used for the field.
             */
            fieldLabel() {
                // If the field name is purposefully an empty string, then let's show it as such
                if (this.fieldName === '') {
                    return ''
                }

                return this.fieldName || this.field.name || this.field.singularLabel
            },

            /**
             * Return the classes that should be used for the field content.
             */
            fieldClasses() {
                return this.fullWidthContent
                    ? this.field.stacked
                        ? 'w-full pt-2 pb-8 px-10'
                        : 'w-4/5 py-6'
                    : this.hasSize
                        ? 'w-full pt-2 pb-8 px-10'
                        : 'w-1/2 py-6'
            },


            /**
             * Return the size that should be used for the field container.
             */
            elementSize() {
                return this.field.size || 'w-full'
            },

            /**
             * Return if the field has a size
             */
            hasSize() {
                return this.field.size !== undefined;
            },

            /**
             * Return if we must to remove or not the bottom border field
             */
            getRemoveBottomBorder() {
                return true
            }
        },
    }
</script>
<style>
.tooltip {
  display: block !important;
  z-index: 10000;
  background: black !important;
  padding: 0px;
}

.tooltip .tooltip-inner {
  background: black;
  color: white;
  border-radius: 16px;
  padding: 5px 10px 4px;

}

.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: black;
  z-index: 1;
}

.tooltip[x-placement^="top"] {
  margin-bottom: 5px;
}

.tooltip[x-placement^="top"] .tooltip-arrow {
  border-width: 5px 5px 0 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  bottom: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}

.tooltip[x-placement^="bottom"] {
  margin-top: 5px;
}

.tooltip[x-placement^="bottom"] .tooltip-arrow {
  border-width: 0 5px 5px 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-top-color: transparent !important;
  top: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}

.tooltip[x-placement^="right"] {
  margin-left: 5px;
}

.tooltip[x-placement^="right"] .tooltip-arrow {
  border-width: 5px 5px 5px 0;
  border-left-color: transparent !important;
  border-top-color: transparent !important;
  border-bottom-color: transparent !important;
  left: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}

.tooltip[x-placement^="left"] {
  margin-right: 5px;
}

.tooltip[x-placement^="left"] .tooltip-arrow {
  border-width: 5px 0 5px 5px;
  border-top-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  right: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}

.tooltip.popover .popover-inner {
  background: #f9f9f9;
  color: black;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, .1);
}

.tooltip.popover .popover-arrow {
  border-color: #f9f9f9;
}

.tooltip[aria-hidden='true'] {
  visibility: hidden;
  opacity: 0;
  transition: opacity .15s, visibility .15s;
}

.tooltip[aria-hidden='false'] {
  visibility: visible;
  opacity: 1;
  transition: opacity .15s;
}


</style>