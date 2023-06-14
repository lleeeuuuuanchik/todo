<template>
	<div class="checkbox">
		<label>
			<input
				class="checkbox__checkbox"
				type="checkbox"
				:checked="value"
				@change="update"
				:disabled="editable"
			>
			<div class="checkbox__checkbox-fake" :class="{'unable': editable}"></div>
		</label>
		<p
			v-if='!editable'
			@click="!value ? editable = true : ''"
			class="checkbox__text"
			:class="{'complete': value}"
		>
			{{ content }}

		</p>

		<input
			class="checkbox__checkbox-edit"
			v-else
			type="text"
			v-model="content"
			@keyup.enter="editContent(doId, content)"
		>
	</div>
</template>

<script>
export default {
	name: 'CheckboxView',

	props:
	{
		title:
		{
			type: String,
			required: true,
		},
		doId:
		{
			type: Number,
		},
		value:
		{
			type: Boolean,
		}
	},

	data()
	{
		return {
			editable: false,
			content: this.title
		};
	},

	methods:
	{
		update(event)
		{
			this.$emit('input', event.target.checked);
		},
		editContent(idx, content)
		{
			this.editable = !this.editable;
			this.$emit('updateContent', {idx, content})
		}
	},
};
</script>

<style lang="scss">
	.checkbox
	{
		display: flex;
		align-items: center;
		gap: 10px;
		margin-bottom: 10px;
	}

	.checkbox__checkbox { display: none; }

	.checkbox__checkbox-fake
	{
		cursor: pointer;
		width: 24px;
		height: 24px;
		background: #F2F2F2;
		border: 1px solid #E0E0E0;
		border-radius: 3px;
		position: relative;

		&:before
		{
			content: '';
			position: absolute;
			width: 22px;
			height: 22px;
			top: -1px;
			left: -1px;
			background: #6FCF97;
			border: 1px solid #27AE60;
			border-radius: 3px;
			background-image: url('@/assets/svg/select.svg');
			background-position: center;
			background-repeat: no-repeat;
			opacity: 0;
		}
	}

	.unable { border: 1px solid red; cursor: auto;}

	.checkbox__checkbox:checked + .checkbox__checkbox-fake:before { opacity: 1; }

	.checkbox__text
	{
		font-weight: 400;
		font-size: 14px;
		line-height: 16px;
		color: #4F4F4F;
	}

	.complete
	{
		font-weight: 400;
		font-size: 14px;
		line-height: 16px;
		text-decoration-line: line-through;
		color: #BDBDBD;
	}

	.checkbox__checkbox-edit
	{
		border: none;
		padding-bottom: 3px;
		width: 100%;
		outline: none;
		border-bottom: 1px solid #313131;
		font-weight: 400;
		font-size: 14px;
		line-height: 16px;
		color: #4F4F4F;
	}
</style>
