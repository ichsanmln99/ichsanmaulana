<template>
	<div>
		<form name="contact" method="POST" class="w-full" data-netlify="true">
			<div class="form-control">
				<label class="label">
					<span class="label-text">Nama</span>
				</label>
				<input
					type="text"
					placeholder="Nama Kamu"
					name="name"
					class="input input-bordered w-full"
				/>
			</div>
			<div class="form-control">
				<label class="label">
					<span class="label-text">Email</span>
				</label>
				<input
					type="text"
					placeholder="Alamat Email"
					name="email"
					class="input input-bordered w-full"
				/>
			</div>
			<div class="form-control">
				<label class="label">
					<span class="label-text">Tulis Pesanmu</span>
				</label>
				<textarea
					class="textarea textarea-bordered placeholder-opacity-30"
					name="message"
					placeholder="Pesan Kamu"
					rows="8"
				></textarea>
			</div>
			<div class="form-control mt-8">
				<button class="btn btn-primary" type="submit">Kirim</button>
			</div>
		</form>
	</div>
</template>

<script>
export default {
	layout: 'layoutMainNavbar',
	data() {
		return {}
	},
}
</script>
