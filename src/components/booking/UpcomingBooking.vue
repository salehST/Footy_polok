<template>
	<div v-if="dates" class="bg-white w-full p-4 rounded-md shadow mb-7">
		<div class="inner-info flex items-start flex-col md:flex-row justify-between gap-4 w-full">
			<div class="img hidden md:block w-52 h-36 min-w-max overflow-hidden rounded">
				<img :src="dates.pitch.location.logo_url" class="object-cover w-full h-full" alt="" />
			</div>
			<div class="text-area w-full">
				<h3 class="text-brand text-2xl font-700 mb-0 flex w-full items-center justify-between">
					{{ dates.pitch.location.name }}
					<button @click="toggleDropdown(1)">
						<img
							src="/img/icons/icon-arrow.svg"
							class="h-32 w-32 flex md:hidden"
							alt="arrow-icon"
						/>
					</button>
				</h3>
				<div v-show="dropdownVisible[1]" class="booking-more-info overflow-hidden">
					<p class="text-brand text-20 font-semibold mb-3">
						{{ dates.display_date }} on Field {{ dates.pitch.name }}
					</p>
					<ul>
						<li class="flex items-center gap-2 justify-start text-brand text-base mb-2">
							<img src="/img/icons/icon-pin.svg" class="h-6 w-6" alt="icon" />
							<span>{{ dates.pitch.location.address }}</span>
						</li>
						<li class="flex items-center gap-2 justify-start text-brand text-base">
							<img src="/img/icons/icon-pitch.svg" class="h-6 w-6" alt="icon" />
							<span
								>Field {{ dates.pitch.name }} ({{ dates.pitch_type.replace('v', ' vs. ') }})</span
							>
						</li>
					</ul>
					<div class="btn-a flex md:hidden w-full min-w-max lg:w-1/4 mt-6 text-right justify-end">
						<a href="" class="org-btn-2">Edit booking</a>
					</div>
				</div>
			</div>
			<div
				class="btn-a hidden md:flex w-full min-w-max lg:w-1/4 text-right justify-start md:justify-end"
			>
				<a href="" class="org-btn-2">Edit booking</a>
			</div>
		</div>
	</div>
	<div v-else>You don't have any bookings yet.</div>
</template>
<script lang="ts">
import Vue from 'vue';
import { mapGetters } from 'vuex';

interface DropdownVisible {
	[key: number]: boolean;
}

export default Vue.extend({
	data() {
		return {
			dropdownVisible: {
				1: false,
				2: false,
			} as DropdownVisible,
		};
	},
	computed: {
		...mapGetters(['getplayerBookings']),
		dates() {
			const currentDate = new Date(); // This will give you the current date and time
			const today = currentDate.toISOString().slice(0, 10); // This will give you the date in the YYYY-MM-DD format
			const upcomingDates = this.getplayerBookings
				.filter((ele: any) => ele.date > today)
				.sort((a: any, b: any) => {
					const dateA: any = new Date(a.date);
					const dateB: any = new Date(b.date);
					return dateA - dateB;
				});
			return upcomingDates[0];
		},
	},

	methods: {
		toggleDropdown(id: number) {
			for (let dropdownId in (this as any).dropdownVisible) {
				if (Number(dropdownId) === id) {
					(this as any).dropdownVisible[id] = !(this as any).dropdownVisible[id];
				} else {
					(this as any).dropdownVisible[Number(dropdownId)] = false;
				}
			}
		},
	},
});
</script>
<style>
@media only screen and (min-width: 767.99px) {
	.booking-more-info {
		display: block !important;
	}
}
</style>
