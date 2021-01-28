<template>
	<section class="table_container">
		<div>
			<button @click="modalOpen()">Добавить</button>

			<table class="table">
				<tr class="table__row">
					<th class="table_column"><div @click="sortByName">Имя</div></th>
					<th class="table_column"><div @click="sortByPhone">Номер телефона</div></th>
				</tr>
				<tr
					class="table__row"
					v-for="(item, index) in items"
					:key="index"
					:class="{ _inferior: item.head != 'none' }"
					@click="showInfo(item)"
				>
					<td class="table_column">{{ item.name }}</td>
					<td class="table_column">{{ item.phone }}</td>
				</tr>
			</table>

			<div class="info_container" v-show="isShowInfo == true">
				<p class="info_name">Имя: {{ info.name }}</p>
				<p class="info_name">Телефон: {{ info.phone }}</p>
				<p class="info_name">Руководитель: {{ info.head }}</p>
			</div>

			<div id="modal" v-show="isModal">
				<form action="/action_page.php" class="add_form" @submit.prevent="addNewPerson()">
					<div class="add_item">
						<label for="fname">Имя</label>
						<input type="text" v-model="newName" id="fname" name="firstname" placeholder="Имя" />
					</div>

					<div class="add_item">
						<label for="lname">Телефон</label>
						<input
							type="number"
							v-model="newPhone"
							id="lname"
							name="phone"
							placeholder="Номер телефона"
						/>
					</div>

					<div class="add_item">
						<label for="country">Начальник</label>
						<select id="name" v-model="newHead" name="name">
							<option value="none">Нет</option>
							<option v-for="(item, index) in items" :key="index" :value="item.name">{{
								item.name
							}}</option>
						</select>
					</div>

					<button type="submit" class="button">Сохранить</button>
				</form>
			</div>
		</div>
	</section>
</template>

<script>
	export default {
		name: 'tableItem',
		data() {
			return {
				isModal: false,
				newName: '',
				newPhone: null,
				newHead: 'none',
				items: [{ name: 'Anna', phone: '89096725085', head: 'none' }],
				isShowInfo: false,
				info: {},
			};
		},
		mounted() {
			if (localStorage.items) {
				this.items = JSON.parse(localStorage.items);
			}
		},
		watch: {
			items() {
				localStorage.items = JSON.stringify(this.items);
			},
		},
		methods: {
			showInfo(item) {
				console.log('asd');
				this.isShowInfo = true;
				this.info = {
					name: item.name,
					phone: item.phone,
					head: item.head,
				};
			},
			sortByName() {
				this.items = this.items.sort((a, b) => a.name.localeCompare(b.name));
			},
			sortByPhone() {
				this.items = this.items.sort((a, b) => a.phone.localeCompare(b.phone));
			},
			addNewPerson() {
				if (this.newName != '' && this.newPhone != null) {
					this.modalClose();

					let staff = this.items.findIndex(this.findHead);

					if (staff != -1) {
						this.items.splice(staff + 1, 0, {
							name: this.newName,
							phone: this.newPhone,
							head: this.newHead,
						});
					} else {
						this.items.push({ name: this.newName, phone: this.newPhone, head: this.newHead });
					}
				}
			},
			findHead(head) {
				if (this.newHead == head.name) {
					console.log(this.newHead);
					return head;
				}
			},
			modalOpen() {
				this.isModal = true;
			},
			modalClose() {
				this.isModal = false;
			},
		},
	};
</script>

<style scoped lang="scss">
	.table_container {
		width: 100%;

    .info_container {
      display: flex;
      align-items: center;
      background: green;
      padding: 1em;

      p {
        color: #fff;
        &+p {
          margin-left: 1em;
        }
      }
    }

		button {
			margin-bottom: 2em;
		}

		.table {
			width: 100%;
			border-collapse: collapse;

			.table__row {
				background: #cccce4;

				th {
					background: #5a5a98;
					color: #fff;
				}

				&._inferior {
					background: none;
				}
				.table_column {
					border: 1px solid #000;
					cursor: pointer;
					width: 50%;
				}
			}
		}

		#modal {
			position: fixed;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			background: #292929bf;
			display: flex;
			align-items: center;
			justify-content: center;

			form {
				display: flex;
				align-items: center;
				justify-content: center;
				flex-direction: column;
				background: #fff;
				padding: 4em;

				.add_item {
					margin-bottom: 1em;
					display: flex;
					align-items: center;
					justify-content: space-between;
					width: 100%;

					label {
						margin: 0;
					}
					input,
					select {
						width: 200px;
						margin-left: 2em;
						height: 30px;
					}
				}
			}
		}
	}
</style>
