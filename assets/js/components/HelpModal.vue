<template>
	<Teleport to="body">
		<div
			id="helpModal"
			class="modal fade text-dark"
			tabindex="-1"
			role="dialog"
			aria-hidden="true"
		>
			<div class="modal-dialog modal-dialog-centered" role="document">
				<div class="modal-content">
					<div class="modal-header">
						<h5 class="modal-title">{{ $t("help.modalTitle") }}</h5>
						<button
							type="button"
							class="btn-close"
							data-bs-dismiss="modal"
							aria-label="Close"
						></button>
					</div>
					<div class="modal-body">
						<p>
							{{ $t("help.primaryActions") }}
						</p>
						<div
							class="d-block d-sm-flex justify-content-between align-items-stretch mb-4"
						>
							<a
								:href="docsUrl"
								target="_blank"
								class="btn btn-outline-primary w-100 w-sm-auto flex-grow-1 mb-3 mb-sm-0 me-sm-3"
								type="button"
							>
								{{ $t("help.documentationButton") }}
							</a>
							<a
								href="https://github.com/evcc-io/evcc/discussions"
								target="_blank"
								class="btn btn-outline-primary w-100 w-sm-auto flex-grow-1"
								type="button"
							>
								{{ $t("help.discussionsButton") }}
							</a>
						</div>
						<hr class="mb-4" />
						<p>
							{{ $t("help.secondaryActions") }}
						</p>
						<div
							class="d-block d-sm-flex justify-content-between align-items-baseline mb-3"
						>
							<p class="flex-sm-grow-1 opacity-50 me-sm-3">
								{{ $t("help.logsDescription") }}
							</p>
							<router-link to="/log" class="btn btn-outline-primary text-nowrap">
								{{ $t("help.logsButton") }}
							</router-link>
						</div>
						<div
							class="d-block d-sm-flex justify-content-between align-items-baseline mb-3"
						>
							<p class="flex-sm-grow-1 opacity-50 me-sm-3">
								{{ $t("help.issueDescription") }}
							</p>
							<a
								href="https://github.com/evcc-io/evcc/issues"
								target="_blank"
								class="btn btn-outline-primary text-nowrap"
							>
								{{ $t("help.issueButton") }}
							</a>
						</div>

						<div
							class="d-block d-sm-flex justify-content-between align-items-baseline mb-3"
						>
							<p class="flex-sm-grow-1 opacity-50 me-sm-3">
								{{ $t("help.restartDescription") }}
							</p>
							<button
								class="btn btn-outline-danger text-nowrap"
								type="button"
								data-bs-dismiss="modal"
								@click="openConfirmRestartModal"
							>
								{{ $t("help.restartButton") }}
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</Teleport>
	<Teleport to="body">
		<div
			id="confirmRestartModal"
			class="modal fade text-dark"
			tabindex="-1"
			role="dialog"
			aria-hidden="true"
		>
			<div class="modal-dialog modal-dialog-centered" role="document">
				<div class="modal-content">
					<div class="modal-header">
						<h5>{{ $t("help.restart.modalTitle") }}</h5>
					</div>
					<div class="modal-body">
						<p>{{ $t("help.restart.description") }}</p>
						<p>
							<small>
								{{ $t("help.restart.disclaimer") }}
							</small>
						</p>
					</div>
					<div class="modal-footer d-flex justify-content-between">
						<button
							type="button"
							class="btn btn-link text-muted"
							data-bs-dismiss="modal"
							@click="openHelpModal"
						>
							{{ $t("help.restart.cancel") }}
						</button>
						<button
							type="button"
							class="btn btn-danger"
							data-bs-dismiss="modal"
							@click="restartConfirmed"
						>
							{{ $t("help.restart.confirm") }}
						</button>
					</div>
				</div>
			</div>
		</div>
	</Teleport>
</template>

<script lang="ts">
import Modal from "bootstrap/js/dist/modal";
import { docsPrefix } from "../i18n";
import { performRestart } from "../restart";
import { isLoggedIn, openLoginModal } from "./Auth/auth";
import { defineComponent } from "vue";

export default defineComponent({
	name: "HelpModal",
	props: {},
	computed: {
		docsUrl() {
			return `${docsPrefix()}/`;
		},
	},
	methods: {
		openHelpModal() {
			const modal = Modal.getOrCreateInstance(
				document.getElementById("helpModal") as HTMLElement
			);
			modal.show();
		},
		openConfirmRestartModal() {
			const modal = Modal.getOrCreateInstance(
				document.getElementById("confirmRestartModal") as HTMLElement
			);
			if (!isLoggedIn()) {
				openLoginModal(null, modal);
			} else {
				modal.show();
			}
		},
		async restartConfirmed() {
			await performRestart();
		},
	},
});
</script>
