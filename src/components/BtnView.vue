<template>
	<div>
		<button>{{ title }}</button>
	</div>
</template>

<script>
export default {
	name: 'BtnView',
	props:
	{
		title:
		{
			type: String,
			required: true,
		}
	},
};
</script>

<style scoped>
	button
	{
		margin-top: 30px;
		cursor: pointer;
		border: none;
		width: 100%;
		padding: 15px 50px;
		background: #27AE60;
		border-radius: 4px;
		color: white;
		transition: 0.2s ease;
	}

	button:hover { background: green; }
</style>
