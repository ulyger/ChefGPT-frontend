<script>
	import { goto } from '$app/navigation';
  import { PUBLIC_BACKEND_BASE_URL } from '$env/static/public';


	let formErrors = {};

	function postSignUp() {
		const successMessage = 'Sign-up successful!';
		window.alert(successMessage);
		goto('./login');
	}

	async function createUser(evt) {
		evt.preventDefault();

    if (evt.target['password'].value.length < 8) {
			formErrors['password'] = { message: 'should be at least 8 characters' };
			return;
		}
		

		if (evt.target['password'].value != evt.target['password-confirmation'].value) {
			formErrors['password'] = { message: 'Password confirmation does not match' };
			return;
		}

		if (evt.target['name'].value.length == 0) {
			formErrors['name'] = { message: 'cannot be blank' };
			return;
		}

		if (evt.target['email'].value.length == 0) {
			formErrors['email'] = { message: 'cannot be blank' };
			return;
		}

		const userData = {
			name: evt.target['name'].value,
			email: evt.target['email'].value,
			password: evt.target['password'].value,
		};

		const resp = await fetch(PUBLIC_BACKEND_BASE_URL + '/users/new', {
			method: 'POST',
			mode: 'cors',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify(userData)
		});

    if (resp.status == 200) {
			postSignUp();
		} else {
			throw 'Sign up failed';
		}
	}
</script>

<div class="flex justify-center items-center mt-8">
	<form on:submit={createUser} class="w-1/3">
		<div class="form-control w-full">
			<label class="label" for="name">
				<span>Username</span>
			</label>
			<input type="text" name="name" placeholder="johndoe" class="input input-bordered w-full" />
			{#if 'name' in formErrors}
				<label class="label" for="name">
					<span class="label-text-alt text-red-500">{formErrors['name'].message}</span>
				</label>
			{/if}
		</div>

		<div class="form-control w-full">
			<label class="label" for="email">
				<span>Email</span>
			</label>
			<input
				type="email"
				name="email"
				placeholder="john@example.com"
				class="input input-bordered w-full"
				required
			/>
			{#if 'email' in formErrors}
				<label class="label" for="email">
					<span class="label-text-alt text-red-500">{formErrors['email'].message}</span>
				</label>
			{/if}
		</div>

		<div class="form-control w-full">
			<label class="label" for="password">
				<span>Password</span>
			</label>
			<input
				type="password"
				name="password"
				placeholder=""
				class="input input-bordered w-full"
				required
			/>
			{#if 'password' in formErrors}
				<label class="label" for="password">
					<span class="label-text-alt text-red-500">{formErrors['password'].message}</span>
				</label>
			{/if}
		</div>

		<div class="form-control w-full">
			<label class="label" for="password">
				<span>Confirm Password</span>
			</label>
			<input
				type="password"
				name="password-confirmation"
				placeholder=""
				class="input input-bordered w-full"
				required
			/>
			{#if 'password' in formErrors}
				<label class="label" for="password">
					<span class="label-text-alt text-red-500">{formErrors['password'].message}</span>
				</label>
			{/if}
		</div>

		

		<div class="form-control w-full mt-4">
			<button class="btn btn-md">Create an Account</button>
		</div>
    <div class="text-center">
			<a class="link-hover italic text-xs" href="/users/login"
				>Already have an account? Click here to login instead.</a
			>
		</div>
	</form>
</div>