<template>
    <div ref="errors">
        <template v-for="error in errors">
            <AnimatedErrorView :key="error.id">
                {{ error.human || error.message }}
            </AnimatedErrorView>
        </template>
    </div>
</template>

<script lang="ts">
import { SimpleError } from '@simonbackx/simple-errors';
import { Component, Prop, Vue, Watch } from "vue-property-decorator";

import { ErrorBox } from "./ErrorBox"
import AnimatedErrorView from "./AnimatedErrorView.vue"

@Component({
    components: {
        AnimatedErrorView
    }
})export default class GeneralErrorsView extends Vue {
    @Prop() errorBox: ErrorBox | null;
    errors: SimpleError[] = [];

    @Watch('errorBox')
    onNewErrors(val: ErrorBox | null ) {
        if (!val) {
            this.errors = [];
            return;
        }
        // Wait for next tick, to prevent a useless rerender of errors that will get removed by other inputs
        this.$nextTick(() => {
            const errors = val.remaining
            this.errors = errors.errors
            val.scrollTo(this.errors, this.$refs.errors as HTMLElement)
        });
        
    }
}
</script>