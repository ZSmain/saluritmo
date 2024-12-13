<script>
	import { Button, buttonVariants } from '$lib/components/ui/button';
	import { ScrollArea } from '$lib/components/ui/scroll-area';
	import * as Drawer from '$lib/components/ui/drawer';

	import { Trash } from 'lucide-svelte';

	let prescriptions = $state([
		{
			date: '2023-05-01',
			medications: [
				{
					name: 'Amoxicillin',
					dci: 'Amoxicillin trihydrate',
					dosage: '500mg',
					startDate: '2023-05-01',
					endDate: '2023-05-15'
				},
				{
					name: 'Ibuprofen',
					dci: 'Ibuprofen',
					dosage: '200mg',
					startDate: '2023-05-01',
					endDate: '2023-05-10'
				}
			]
		},
		{
			date: '2023-04-15',
			medications: [
				{
					name: 'Atorvastatin',
					dci: 'Atorvastatin calcium',
					dosage: '10mg',
					startDate: '2023-04-15',
					endDate: '2023-10-15'
				}
			]
		},
		{
			date: '2023-03-30',
			medications: [
				{
					name: 'Metformin',
					dci: 'Metformin hydrochloride',
					dosage: '500mg',
					startDate: '2023-03-30',
					endDate: '2023-09-30'
				},
				{
					name: 'Lisinopril',
					dci: 'Lisinopril',
					dosage: '10mg',
					startDate: '2023-03-30',
					endDate: '2023-09-30'
				}
			]
		}
	]);
	let showDrawer = $state(false);
	let currentPrescription = null;

	const handleAddPrescription = () => {
		currentPrescription = null;
		showDrawer = true;
	};

	const handleModifyPrescription = (index) => {
		currentPrescription = prescriptions[index];
		showDrawer = true;
	};

	const handleSavePrescription = (prescription) => {
		if (currentPrescription) {
			const index = prescriptions.indexOf(currentPrescription);
			prescriptions[index] = prescription;
			prescriptions = prescriptions;
		} else {
			prescriptions = [...prescriptions, prescription];
		}
		showDrawer = false;
		currentPrescription = null;
	};

	const handleCloseDrawer = () => {
		showDrawer = false;
		currentPrescription = null;
	};
</script>

<Drawer.Root bind:open={showDrawer}>
	<Drawer.Content>
		<div class="mx-auto w-full max-w-sm">
			<Drawer.Header>
				<Drawer.Title
					>{currentPrescription ? 'Edit Prescription' : 'Add New Prescription'}</Drawer.Title
				>
				<Drawer.Description>
					{currentPrescription
						? 'Update your prescription details'
						: 'Add a new prescription and medications'}
				</Drawer.Description>
			</Drawer.Header>
			<div>
				<form
					onsubmit={(e) => {
						e.preventDefault();
						const formData = new FormData(e.target);
						const prescription = {
							date: formData.get('date'),
							medications: Array.from(formData.getAll('medication')).map((medication) => {
								const [name, dci, dosage, startDate, endDate] = medication.split('|');
								return {
									name,
									dci,
									dosage,
									startDate,
									endDate
								};
							})
						};
						handleSavePrescription(prescription);
					}}
				>
					<div class="grid gap-4">
						<div class="grid gap-2">
							<label for="date">Prescription Date</label>
							<input type="date" id="date" name="date" value={currentPrescription?.date || ''} />
						</div>
						<div class="grid gap-2">
							<label for="medication">Medications</label>
							{#each currentPrescription?.medications || [] as medication, index}
								<div class="flex items-center gap-2">
									<input
										type="text"
										id={`medication-${index}`}
										name="medication"
										value={`${medication.name}|${medication.dci}|${medication.dosage}|${medication.startDate}|${medication.endDate}`}
									/>
									<Button
										type="button"
										variant="ghost"
										size="icon"
										onclick={() => {
											const updatedMedications = [...(currentPrescription.medications || [])];
											updatedMedications.splice(index, 1);
											currentPrescription = {
												...currentPrescription,
												medications: updatedMedications
											};
										}}
									>
										<Trash class="h-5 w-5" />
									</Button>
								</div>
							{/each}
							<Button
								type="button"
								variant="outline"
								onclick={() => {
									currentPrescription = {
										...currentPrescription,
										medications: [
											...(currentPrescription?.medications || []),
											{
												name: '',
												dci: '',
												dosage: '',
												startDate: '',
												endDate: ''
											}
										]
									};
								}}
							>
								Add Medication
							</Button>
						</div>
					</div>
					<Drawer.Footer class="pt-2">
						<Drawer.Close class={buttonVariants({ variant: 'outline' })}>Cancel</Drawer.Close>
					</Drawer.Footer>
				</form>
			</div>
		</div>
	</Drawer.Content>
</Drawer.Root>

<div class="flex flex-col h-full">
	<header class="bg-primary text-primary-foreground px-4 py-2 flex items-center justify-between">
		<div>
			<span class="font-medium text-lg">Prescriptions & Medications</span>
		</div>
	</header>
	<section class="flex flex-col flex-1 p-4 sm:p-6 overflow-auto">
		<div class="flex items-center justify-between mb-4">
			<h2 class="text-lg font-medium">Medications</h2>
			<Button
				variant="outline"
				class="bg-primary text-primary-foreground"
				onclick={handleAddPrescription}
			>
				Add New Prescription
			</Button>
		</div>

		<ScrollArea class="flex-1 w-full rounded-md border p-4">
			<div class="grid gap-4">
				{#each prescriptions as prescription, index}
					<div class="border rounded-md p-4">
						<div class="font-medium">
							Prescription - {prescription.date}
						</div>
						<div class="grid gap-2 mt-2">
							{#each prescription.medications as medication, i}
								<div class="grid gap-1">
									<div class="flex items-center justify-between">
										<div class="font-medium">
											{medication.name}
										</div>
										<div class="text-muted-foreground">
											{medication.dosage}
										</div>
									</div>
									<div class="flex items-center justify-between text-sm text-muted-foreground">
										<div>{medication.dci}</div>
										<div>
											{medication.startDate} - {medication.endDate}
										</div>
									</div>
								</div>
							{/each}
						</div>
						<div class="flex justify-end mt-2">
							<Button variant="outline" onclick={() => handleModifyPrescription(index)}>
								Modify
							</Button>
						</div>
					</div>
				{/each}
			</div>
		</ScrollArea>
	</section>
</div>
