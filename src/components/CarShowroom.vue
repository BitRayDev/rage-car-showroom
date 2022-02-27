<template>
	<div class="showroom">
		<div class="purchase-modal" v-if="isSubmitModalActive">
			<div class="purchase-modal__top-border"></div>
			<p class="purchase-modal__text">Do you really want to buy this car?</p>
			<div class="purchase-modal__buttons">
				<button class="purchase-modal__button purchase-modal__button_type_submit" @click="onSubmitButtonClicked">Yes</button>
				<button class="purchase-modal__button purchase-modal__button_type_cancel" @click="onRefuseButtonClicked">No</button>
			</div>
		</div>
		<div class="showroom__section">
			<div>
				<p class="showroom__title">Car Showroom</p>
				<div class="showroom__splitter"></div>
				<p class="showroom__subtitle">here you can buy a car or make a test drive</p>
			</div>
			<div class="showroom__cars-list-container">
				<div class="showroom__cars-list">
					<div class="car" v-for="car in cars" @click="() => selectCar(car)">
						<div
							class="car__producer-logo-container"
							:style="{backgroundColor: car == selectedCar ? '#FFC700' : '#505050'}"
						>
							<img class="car__producer-logo" :src="car.producer.logo" />
						</div>
						<p
							class="car__model"
							:style="{color: car == selectedCar ? '#FFC700' : '#989898'}"
						>{{car.model}}</p>
						<p class="car__price" :style="{color: car == selectedCar ? 'white' : '#505050'}">
							<svg
								class="car__label-icon"
								width="14"
								height="14"
								viewBox="0 0 14 14"
								fill="none"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									d="M13.3614 0.283522C13.3208 0.182381 13.2428 0.10079 13.1435 0.0557227C13.0443 0.0106559 12.9315 0.00558328 12.8286 0.0415602C12.7258 0.0775371 12.6407 0.151793 12.5912 0.248881C12.5416 0.345969 12.5315 0.458414 12.5627 0.562822C13.213 2.43182 11.9222 3.82762 10.938 4.57522L10.5404 4.00542C10.4067 3.81432 10.1078 3.65542 9.87543 3.65262L7.64383 3.66242C7.37122 3.67066 7.1059 3.7524 6.87593 3.89902L0.300128 8.50992C0.147175 8.61756 0.0431018 8.78141 0.0106962 8.96561C-0.0217095 9.14981 0.0201927 9.33935 0.127228 9.49272L3.11553 13.7683C3.33953 14.0868 3.69723 14.0483 4.01643 13.8257L10.5922 9.21412C10.7812 9.08042 10.9996 8.79202 11.0766 8.57152L11.7752 6.37002C11.8522 6.15022 11.8053 5.81492 11.6716 5.62382L11.4294 5.27662C12.751 4.26372 14.123 2.47382 13.3614 0.283522V0.283522ZM10.3017 6.83412C10.1806 6.91892 10.0439 6.97902 9.89955 7.01098C9.75517 7.04294 9.60591 7.04615 9.46029 7.0204C9.31468 6.99466 9.17557 6.94048 9.0509 6.86095C8.92623 6.78143 8.81846 6.67812 8.73373 6.55692C8.56218 6.31168 8.49494 6.00839 8.54678 5.71363C8.59862 5.41887 8.7653 5.15672 9.01023 4.98472C9.20224 4.85019 9.43149 4.77902 9.66594 4.78116C9.90038 4.78329 10.1283 4.85862 10.3178 4.99662C10.1274 5.11142 9.99653 5.17862 9.97203 5.18912C9.88526 5.2305 9.81511 5.30012 9.77308 5.38657C9.73106 5.47303 9.71964 5.5712 9.74071 5.665C9.76178 5.75879 9.81408 5.84265 9.88905 5.90283C9.96401 5.963 10.0572 5.99593 10.1533 5.99622C10.2135 5.99622 10.2751 5.98222 10.3332 5.95492C10.469 5.89052 10.6146 5.81422 10.7665 5.72392C10.8015 5.93397 10.7764 6.14961 10.6942 6.34602C10.6119 6.54243 10.4759 6.71162 10.3017 6.83412V6.83412Z"
									:fill="car == selectedCar ? 'white' : '#505050'"
								/>
							</svg>
							{{car.price}}$
						</p>
						<img class="car__image" :src="car.image" />
					</div>
				</div>
			</div>
			<div class="showroom__buttons">
				<button class="showroom__buy-button" @click="onBuyButtonClicked">Buy Car</button>
				<button class="showroom__testdrive-button" @click="onTestDriveButtonClicked">Test-Drive</button>
			</div>
		</div>
		<div class="showroom__section showroom__section_align_right">
			<p class="showroom__quit-hint">
				press
				<span class="tag">esc</span>
				to exit
			</p>
			<div class="showroom__balance-container">
				<p class="showroom__balance-label">your balance</p>
				<p class="showroom__balance-value">$ {{balance}}</p>
			</div>
			<div class="showroom__specs-container">
				<p class="showroom__specs-label">Technical specifications</p>
				<div class="showroom__splitter"></div>
				<div class="showroom__specs-list">
					<div class="spec" v-for="(value, name) in selectedCar.specs">
						<div class="spec__text">
							<p class="spec__label">{{specsData[name].label}}</p>
							<p class="spec__value">{{`${value} ${specsData[name].measurementUnit}`}}</p>
						</div>
						<div class="spec__bar">
							<div
								class="spec__bar-thumb"
								:style="{width: `${calculateBarThumb(value, specsData[name].limit)}%`}"
							></div>
						</div>
					</div>
				</div>
			</div>
			<div class="showroom__palette-container">
				<p class="showroom__palette-label">choose car color</p>
				<div class="palette">
					<div
						class="palette__color"
						v-for="color in colors"
						:style="{background: color}"
						:class="{'palette__color_active': color == selectedColor}"
						@click="() => selectColor(color)"
					></div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		specsData: Object,
		cars: Array,
		colors: Array,
		balance: Number,
		colors: Array,
		onPurchaseSubmitted: Function,
		onPurchaseRefused: Function,
		onCarSelected: Function,
		onTestDrive: Function,
	},
	data: function () {
		return {
      isSubmitModalActive: false,
			selectedCar: {},
			selectedColor: "", 
		};
	},
	methods: {
		selectCar: function (car) {
			this.selectedCar = car;
      this.onCarSelected(car);
		},
		selectColor: function (color) {
			this.selectedColor = color;
      this.onColorSelected(color);
		},
    onSubmitButtonClicked: function(e) {
      this.isSubmitModalActive = false;
      this.onPurchaseSubmitted();
    },
    onRefuseButtonClicked: function(e) {
      this.isSubmitModalActive = false;
      this.onPurchaseRefused();
    },
    onTestDriveButtonClicked: function(e) {
      this.onTestDrive();
    },
    onBuyButtonClicked: function(e) {
      this.isSubmitModalActive = true;
    },
		calculateBarThumb(value, maxValue) {
			if (value > maxValue) value = maxValue;
			else if (value < 0) value = 0;

			return (100 / maxValue) * value;
		},
	},
	mounted: function () {
		this.selectedCar = this.cars[0];
		this.selectedColor = this.colors[0];
	},
};
</script>

<style lang="scss">
.showroom {
	display: flex;
	justify-content: space-between;

	width: 100vw;
	height: 100vh;

	box-sizing: border-box;
	padding: 3vw;
}

.showroom__section {
	display: flex;
	flex-direction: column;
	justify-content: space-between;

	height: 100%;
}

.showroom__section_align_right {
	align-items: flex-end;
}

.showroom__title {
	color: white;

	font-size: 2.5vw;
	font-weight: 700;
}

.showroom__subtitle {
	color: white;

	font-size: 1vw;
	font-weight: 400;
}

.showroom__splitter {
	width: 13vw;
	height: 0.2vw;
	background-color: #ffc700;
	margin: 0.5vw -1vw;
}

.showroom__cars-list-container {
	direction: rtl;
	overflow: auto;

	width: 18vw;
	height: 20vw;

	padding: 1vw 2vw;

	&::-webkit-scrollbar {
		background: rgba(255, 255, 255, 0.35);
		border-radius: 0.2vw;

		width: 0.3vw;
	}

	&::-webkit-scrollbar-thumb {
		background: #ffc700;
		box-shadow: 0px 0px 0.25vw #ffc700;
		border-radius: 0.2vw;

		width: 0.5vw;
	}
}
.showroom__cars-list {
	direction: ltr;

	display: flex;
	flex-direction: column;

	gap: 1.5vw;
}

.car {
	display: flex;
	flex-direction: column;
	justify-content: center;

	position: relative;

	color: white;

	width: 12vw;

	background: #212121;

	box-sizing: border-box;
	padding: 0.3vw 2vw;

	cursor: pointer;
}

.car__producer-logo-container {
	display: flex;
	align-items: center;
	justify-content: center;

	position: absolute;

	width: 2.5vw;
	height: 2.5vw;

	border-radius: 100vw;

	left: -1vw;
	top: -0.5vw;
}
.car__producer-logo {
	width: 60%;
	max-height: 60%;
}

.car__model {
	font-size: 1.1vw;
	width: 7vw;
}

.car__price {
	font-size: 0.8vw;
}
.car__label-icon {
	height: 0.6vw;
}

.car__image {
	position: absolute;
	height: 4vw;

	left: 10vw;
	top: 50%;

	transform: translate(0, -50%);
}

.showroom__buttons {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.showroom__buy-button {
	font-size: 1.2vw;
	font-weight: 700;

	box-shadow: 0px 0px 0.5vw #ffc700;

	border: none;
	border-radius: 0.25vw;

	width: 13vw;
	height: 4vw;

	background: #ffc700;
}

.showroom__testdrive-button {
	color: #989898;

	font-size: 0.8vw;

	border: none;

	width: 9vw;
	height: 3vw;

	background: #393939;
}

.showroom__quit-hint {
	color: white;
	font-size: 1vw;
}

.tag {
	color: black;

	padding: 0.25vw 0.5vw;

	border-radius: 0.5vw;

	background: #ffc700;
}

.showroom__balance-container {
}
.showroom__balance-label {
	font-size: 1.2vw;
	color: #a8a8a8;
}
.showroom__balance-value {
	font-size: 2.5vw;
	font-weight: 700;

	color: white;
}

.showroom__specs-label {
	font-size: 1.2vw;
	font-weight: 700;

	color: white;
}

.showroom__specs-list {
	display: flex;
	flex-direction: column;

	gap: 1vw;
}

.spec {
	color: white;
	flex-shrink: 0;
	width: 18vw;
}
.spec__text {
	display: flex;
	justify-content: space-between;

	font-size: 0.9vw;
}
.spec__bar {
	height: 0.4vw;

	border-radius: 0.2vw;
	background: rgba(255, 255, 255, 0.22);
}
.spec__bar-thumb {
	background: #ffc700;
	height: 100%;
	border-radius: 0.5vw;
}

.showroom__palette-container {
	display: flex;
	flex-direction: column;
	gap: 0.5vw;

	width: 17vw;
}
.showroom__palette-label {
	color: white;
	font-size: 1.2vw;
}

.palette {
	display: flex;
	flex-flow: row wrap;
	gap: 0.5vw;

	align-self: flex-end;

	width: 15vw;
}

.palette__color {
	border-radius: 0.25vw;
	width: 2vw;
	height: 2vw;
}

.palette__color_active {
	box-shadow: 0px 0px 1vw rgba(255, 255, 255, 0.7);
}

.purchase-modal {
	position: absolute;

	left: 50vw;
	bottom: 5vw;
	transform: translate(-50%, 50%);

	width: 20vw;
	height: 5vw;

	background: #2d2d2d;
	border-radius: 0.3vw;
}

.purchase-modal__top-border {
	height: 0.4vw;

	background: #ffc700;
	border-radius: 0.3vw;
}

.purchase-modal__text {
	color: white;
	text-align: center;
	font-size: 1vw;

	margin-top: 1.5vw;
}

.purchase-modal__buttons {
	display: flex;
	justify-content: center;
  gap: 1vw;

  position: relative;
  bottom: -.5vw;
}

.purchase-modal__button {
  height: 2.5vw;
  width: 6vw;

  font-weight: 600;

  border: none;
	border-radius: .3vw;

	background: white;
}
.purchase-modal__button_type_submit {
	background: #77ff61;
}
.purchase-modal__button_type_cancel {
	background: #FF6161;
}
</style>