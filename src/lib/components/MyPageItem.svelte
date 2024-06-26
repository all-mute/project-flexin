<script>
	import { fade } from 'svelte/transition';
	import PageCardBadge from '$lib/components/PageCardBadge.svelte';
	import { enhance } from '$app/forms';
	import { Modal } from '$lib/components';
	import { getImageURL, getFormattedDateTime } from '$lib/utils';
	import toast from 'svelte-french-toast';
	import MyPageItem from '$lib/components/MyPageItem.svelte';

	import { Icon, PencilSquare, Trash, Check, ArrowPathRoundedSquare, EyeSlash } from 'svelte-hero-icons';

	export let page;
	export let user;
	export let localUser;

	const formattedDateTime = getFormattedDateTime(page.updated);
	export let isPrivate = page.private;

	// let modalOpen;
	let loading = false;

	const openModal = () => {
		modalOpen = true;
	};

	const submitDeletePage = () => {
		loading = true;
		return async ({ result, update }) => {
			if (result.type === 'success') {
				toast.success('Page deleted successfully!');
				await update();
			} else {
				toast.error('Could not delete page. Try again later.');
			}

			loading = false;
		};
	};

	$: modalOpen = false;

</script>

<div
	in:fade
	class="flex w-full rounded border border-neutral/10 transition-all duration-100 hover:border-neutral/50 hover:shadow-lg group"
>
	<div class="w-full flex gap-0 items-center rounded">
		<div class="w-full h-full p-2">
			<a href="/pages/{page.id}" class="">
				<div class="flex justify-between h-full m-1">
					<div>

						<div class="badge badge-sm badge-neutral rounded capitalize font-medium text-white my-1 py-3">{page.company}</div>
						<div class="badge badge-sm badge-secondary border border-secondary rounded capitalize my-1 py-3">{page.domain}</div>
						<div class="badge badge-sm badge-ghost rounded capitalize my-1 py-3">{page.grade}</div>
						<div class="text-neutral font-bold mt-2">
							{page.name.length > 23
								? page.name.slice(0, 23) + '...'
								: page.name
							}
						</div>
						<div class=" text-sm text-base-content/75">
							{page.tagline.length > 26
								? page.tagline.slice(0, 26) + '...'
								: page.tagline
							}
						</div>
					</div>

					<div class="flex flex-col justify-between items-end m-1">
						{#if isPrivate}
							<div class="flex items-strech">
								<PageCardBadge {isPrivate} />
							</div>
						{:else if user}
							<div class="flex items-stretch">
								<img
									class="w-8 h-8 object-cover rounded-full border border-neutral group-hover:saturate-150 transition-color duration-300"
									src={user?.avatar
										? getImageURL(user?.collectionId, user?.id, user?.avatar)
										: `https://ui-avatars.com/api/?name=${user?.name}`}
									alt="User avatar"
								/>

							</div>
						{/if}

						<div class="font-bold text-xs">
							<div class="grid place-items-end">
							{#if page.user === localUser.id}
								<div class="col-start-1 row-start-1 group-hover:invisible">
									<div class="flex gap-1">
										<Icon src={ArrowPathRoundedSquare} class="w-4 h-4" />
										<div class="font-medium text-base-content/75">{formattedDateTime}</div>
									</div>
								</div>

								<div class="col-start-1 row-start-1">
									<div
									class="opacity-0 group-hover:opacity-100 p-1"
								>
										<div class="h-full">
											<div
												class="gap-2 flex justify-end"
											>
												<a href="/pages/{page.id}/edit" class="">
													<Icon
														src={PencilSquare}
														class="w-4 h-4 md:w-5 md:h-5 text-black-100 hover:text-warning"
													/>
												</a>
			
											</div>
										</div>
									</div>
									
								</div>
							{:else}
								<div class="col-start-1 row-start-1">
									<div class="flex gap-1">
										<Icon src={ArrowPathRoundedSquare} class="w-4 h-4" />
										<div class="font-medium text-base-content/75">{formattedDateTime}</div>
									</div>
								</div>
							{/if}
						</div>
					</div>
				</div>
			</a>
		</div>
	</div>
</div>
