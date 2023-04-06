<script lang="ts">
	let handle: string;
	let password: string;
	let email: string;
	let inviteCode: string;

	$: isValid = email && password && handle && inviteCode && inviteCode.match(/bsky.social-\w+/);

	const onClick = async () => {
		if (!isValid) {
			return;
		}

		try {
			const res = await fetch('https://bsky.social/xrpc/com.atproto.server.createAccount', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					handle: `${handle}.bsky.social`,
					password,
					email,
					inviteCode
				})
			});
			const json = await res.json();

			if (res.status >= 400) {
				alert(`${json.error}: ${json.message}`);
			} else {
				alert('Success!');
			}
		} catch (e) {
			alert(e);
		}
	};
</script>

<h1>Bluesky Account Creator</h1>
<span>for android user</span>

<form class="mt-8 flex-col space-y-8">
	<div class="space-y-4">
		<label class="label">
			email
			<input class="input" type="email" placeholder="foo@example.com" bind:value={email} required />
		</label>

		<label class="label">
			password
			<input class="input" type="password" bind:value={password} required />
		</label>

		<label class="label">
			handle
			<div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
				<div class="input-group-shim"><p>@</p></div>
				<input class="input" type="text" placeholder="jack" bind:value={handle} required />
				<div>.bsky.social</div>
			</div>
		</label>

		<label class="label">
			code
			<input
				class="input"
				type="text"
				placeholder="bsky.social-XXXXX"
				pattern="bsky\.social-\w+"
				bind:value={inviteCode}
				required
			/>
		</label>
	</div>

	<button class="btn bg-primary-600" type="submit" on:click={onClick} disabled={!isValid}
		>Try</button
	>
</form>
