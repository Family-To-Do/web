<template>
	<div class="users">
		<div class="users-list">
			<UserItem
				v-for="user in users"
				:key="user.id"
				:user="user"
			/>
		</div>
		<div
			v-if="!showAll"
			class="show-more"
			@click="onClickShowAll">
			<span>Show more</span> <BaseIcon name="expand_more" />
		</div>
	</div>
</template>

<script>
import UserItem from './Item'

export default {
	components: {
		UserItem
	},
	props: {
		expectMe: {
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			showAll: false
		}
	},
	computed: {
		currentUser() {
			return this.$store.state.profile.user
		},
		users() {
			let users = this.$store.state.users.list

			if (!users || !users.length || !this.currentUser) {
				return []
			}

			if (this.expectMe) {
				users = users.filter(user => user.id !== this.currentUser.id)
			}

			if (!this.showAll) {
				return users.slice(0, 3)
			}

			return users
		},
		loading() {
			return this.$store.state.users.loading
		}
	},
	methods: {
		onClickShowAll() {
			this.showAll = true
		}
	}
}
</script>

<style lang="scss" scoped>
.show-more {
	display: flex;
	align-items: center;
	justify-content: flex-end;
	color: #c0c0c0;
	font-size: .7rem;
	line-height: 1;
	cursor: pointer;
	> .base-icon {
		margin-left: 5px;
		margin-top: 2px;
	}
}
</style>
