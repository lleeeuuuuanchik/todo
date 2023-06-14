<template>
	<div id="app">
		<div class="todo">
			<h1 class="todo__title">Список задач</h1>
			<input
				type="text"
				class="todo__input"
				v-model="searchText"
				placeholder="Задача найдется! Просто напиши сюда..."
			>
			<div class="todo__btn-wr">
				<button
					class="todo__btn"
					:class="{'todo__btn--active': filterBy === 'all'}"
					@click="filterBy = 'all'"
				>
					Все задачи
				</button>
				<button
					class="todo__btn"
					:class="{'todo__btn--active': filterBy === 'checked'}"
					@click="filterBy = 'checked'"
				>
					Законченные задачи
				</button>
			</div>
			<div v-for="el in filteredArray" :key="el.id">
				<h2 class="todo__date">{{ getDay(el.date) }}</h2>
				<template v-for="item in el.tasks">
					<checkbox-view
						:key="item.id"
						:title="item.title"
						:doId="item.id"
						v-model="item.checked"
						@updateContent="updateContent"
					/>
				</template>
			</div>
			<btn-view
				@click.native="addDo"
				title="Добавить задачу"
			/>
		</div>
	</div>
</template>

<script>
import CheckboxView from "./components/CheckboxView.vue"
import BtnView from "./components/BtnView.vue"

export default {
	name: 'App',
	components:
	{
		CheckboxView,
		BtnView,
	},
	data()
	{
		const today = new Date()
		const yesterday = new Date(today)
		const twoDaysOff = new Date(today)

		yesterday.setDate(yesterday.getDate() - 1)
		twoDaysOff.setDate(twoDaysOff.getDate() - 2)
		return {
			todo:
			[
				{
					id: 1,
					date: twoDaysOff,
					tasks:
					[
						{ id: 2, title: "complete homeWork", checked: true },
						{ id: 3, title: "complete o2", checked: false },
					]
				},
				{
					id: 4,
					date: yesterday,
					tasks:
					[
						{ id: 5, title: "купить черешню", checked: true },
						{ id: 6, title: "угостить геора", checked: false },
					]
				},
				{
					id: 7,
					date: new Date(),
					tasks:
					[
						{ id: 8, title: "Сьесть себя", checked: false },
						{ id: 9, title: "Покушать", checked: false },
					]
				},
			],
			monthsArray:
			[
				'Январь',
				'Февраль',
				'Март',
				'Апрель',
				'Май',
				'Июнь',
				'Июль',
				'Август',
				'Сентябрь',
				'Ноябрь',
				'Декабрь',
			],

			filterBy: 'all', // checked
			searchText: '',
		}
	},
	computed:
	{
		filteredArray()
		{
			let searchText = this.searchText;
			const showCheckeds = this.filterBy === 'checked';

			if (showCheckeds || searchText)
			{
				const output = [];

				for (const day of this.todo)
				{
					let filtredItems = [...day.tasks];

					if (showCheckeds)
						filtredItems = filtredItems.filter(el => el.checked);

					if (searchText)
						filtredItems = filtredItems.filter(el => el.title.toLowerCase().includes(searchText));

					if (filtredItems.length)
						output.push({
							id: day.id,
							date: day.date,
							tasks: [...filtredItems],
						})
				}

				return output;
			}

			return this.todo;
		},
	},
	methods:
	{
		updateContent({ idx, content })
		{
			let parentId = null;
			let childId = null;
			for (let item of this.todo)
				for (let child of item.tasks)
					if ( child.id === idx )
					{
						childId = child.id;
						parentId = item.id
					}

			const arrayParentId = this.todo.findIndex(el => el.id === parentId);
			const arrayChildId = this.todo[arrayParentId].tasks.findIndex(el => el.id === childId);
			this.todo[arrayParentId].tasks[arrayChildId].title = content;
		},
		addDo()
		{
			let date = new Date();
			for (let item of this.todo)
			{
				if (`${date.getDate()} ${this.monthsArray[date.getMonth()]} ${date.getFullYear()}` === `${item.date.getDate()} ${this.monthsArray[item.date.getMonth()]} ${item.date.getFullYear()}`)
				{
					item.tasks.push({ id: Date.now(), title: 'Новая задача', checked: false })
					break
				}
				else if ( this.todo.find(el => !`${date.getDate()} ${this.monthsArray[date.getMonth()]} ${date.getFullYear()}` === `${el.date.getDate()} ${this.monthsArray[el.date.getMonth()]} ${el.date.getFullYear()}`) )
					this.todo.push({ id: Date.now(), date: new Date(), tasks: [{ id: Date.now(), title: 'Новая задача', checked: false }] })
			}
		},
		getDay(date)
		{
			const currentDay = new Date;
			const yesterday = new Date(currentDay);
			yesterday.setDate(yesterday.getDate() - 1);

			let day = '';

			if (date.getDate() == currentDay.getDate())
				day = 'Сегодня';
			else if (date.getDate() === yesterday.getDate())
				day = 'Вчера';
			return `${day}  ${date.getDate()} ${this.monthsArray[date.getMonth()]} ${date.getFullYear()}`
		},
	},
}
</script>

<style>
@import "./assets/styles/index.css"
</style>
