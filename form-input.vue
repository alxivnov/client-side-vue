<template functional>
	<div :class="[
		data.attrs && data.attrs.type == 'file'
			? 'custom-file'
//			: 'form-group',
//			: 'mb-3',
			: null,
		props.row !== undefined
			? 'row'
			: props.col !== undefined
				? 'col' + (props.col ? '-' + props.col : '')
				: null
	]">
		<label :class="[
			props.row !== undefined
				? 'col-form-label' + (props.size ? '-' + props.size : '')
				: 'form-label',
			props.row !== undefined
				? 'col-' + props.row
				: null,

			data.attrs && data.attrs.type == 'file'
				? 'custom-file-label'
				: null,
		]" :for="data.attrs && data.attrs.id || data.model && data.model.expression" v-show="slots().default">
			<slot></slot>

			<i v-show="data.attrs && data.attrs.required !== undefined" class="fas fa-asterisk text-danger" style="font-size: 1ex"></i>
		</label>

		<input
			v-bind="{
				...data.attrs,

//				Vue 3
				...(data.attrs && data.attrs.modelValue !== undefined
					? {
						value: data.attrs.modelValue,
						onInput: $event => $emit('update:modelValue', $event.target.value)
					}
					: {})
			}"

			v-on="{
				...listeners,
				...(data.model
					? { input: $event => (Array.isArray(listeners.input) ? listeners.input : [ listeners.input ]).forEach(el => el($event.target.value)) }
					: {})
			}"
			:id="data.attrs && (data.attrs.id || data.attrs.name) || data.model && data.model.expression"
			:name="data.attrs && (data.attrs.name || data.attrs.id) || data.model && data.model.expression"

			:class="[
				data.attrs && data.attrs.type == 'file'
//					? 'form-control-file'
					? 'custom-file-input form-control'
					: data.attrs && data.attrs.type == 'range'
//						? 'form-control-range'
						? 'custom-range'
						: props.plaintext
							? 'form-control-plaintext'
							: 'form-control',
				props.size
					? 'form-control-' + props.size
					: null,
				props.row !== undefined
					? 'col'
					: null,

				data.attrs && data.attrs['is-invalid']
					? 'is-invalid'
					: null
			]"

			:type="data.attrs && data.attrs.type || 'text'"
		>
		<small v-show="slots().text" class="form-text text-muted">
			<slot name="text"></slot>
		</small>
		<div v-show="slots().valid" :class="[ props.feedbackTooltips ? 'valid-tooltip' : 'valid-feedback' ]">
			<slot name="valid"></slot>
		</div>
		<div v-show="slots().invalid" :class="[ props.feedbackTooltips ? 'invalid-tooltip' : 'invalid-feedback' ]">
			<slot name="invalid"></slot>
		</div>
	</div>
</template>

<script>
export default {
	inheritAttrs: false,

	props: [
		'col',					// auto|...
		'row',					// true|FALSE

		'size',					// lg|sm
		'plaintext',			// FALSE|true

		'feedback-tooltips'		// FALSE|true
	]
	//	readonly
	//	type
	//	data-browse
}
</script>
