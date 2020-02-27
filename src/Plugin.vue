<template>
	<div>
		<select class="uk-width-1-1" v-model="model.selected">
			<option value />
			<option
				v-bind:value="deepFind(item, options.reference)"
				v-bind:key="deepFind(item, options.reference)"
				v-for="item in components"
			>{{ `${deepFind(item, options.name) || '-'} (${item._uid})` }}</option>
		</select>
	</div>
</template>

<script>
import { PLUGIN_NAME } from './const';

export default {
	mixins: [window.Storyblok.plugin],
	data() {
		return {
			components: []
		};
	},
	methods: {
		getDefaults() {
			return {
				plugin: PLUGIN_NAME,
				version: 'published',
				type: '',
				reference: '_uid',
				name: '',
				selected: ''
			};
		},
		getComputed() {
			// console.log('sbe-component-selector', 'plugin:computed', this.options);
			// defaults
			const defaults = this.getDefaults();

			// constraints checks are left to the browser
			const computed = {
				...defaults,
				version: this.options.version,
				type: this.options.type,
				reference: this.options.reference,
				name: this.options.name
			};

			// return the computed
			return computed;
		},
		initWith() {
			// console.log('sbe-component-selector', 'plugin:init', this.options);
			// if options are initialized
			if (this.options) return this.getComputed();

			// otherwise the defaults
			return this.getDefaults();
		},
		deepFind(object, path) {
			return path.split('.').reduce((a, v) => a[v], object);
		},
		async pluginCreated() {
			// console.log('sbe-component-selector', 'plugin:created', this.options);
			// get this story data
			const res = await this.api.get(`cdn/stories/${this.storyId}`, {
				token: this.token,
				version: this.options.version
			});
			// get the story
			const { story } = res.data;
			// filter the story for the given components
			const components = this.deepFind(story.content, this.options.path)
				.filter(component => {
					return this.options.type
						? this.options.type.split(',').includes(component.component)
						: true;
				})
				.filter(component => this.deepFind(component, this.options.reference));
			// assign the components
			this.components = components;
			// console.log(story, components);
		}
	},
	watch: {
		model: {
			handler(value) {
				this.$emit('changed-model', value);
			},
			deep: true
		}
	}
};
</script>

<style></style>
