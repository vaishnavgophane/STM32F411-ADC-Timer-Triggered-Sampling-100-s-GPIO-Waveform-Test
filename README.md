## 🧑‍💻 Author

**Vaishnav Gophane**  
Embedded Firmware & IoT Developer
<br>
Pune, India.

📫 **Connect:** [Gmail]( mr.vaishnavgophane@gmail.com ) • [GitHub](https://github.com/vaishnavgophane) • [LinkedIn](https://www.linkedin.com/in/vaishnav-gophane-417686284/)

---
# STM32F411 ADC Timer Triggered Sampling (100µs)

This project demonstrates **precisely timed ADC sampling** using **TIM2 TRGO** as an external trigger source on the **STM32 Nucleo-F411RE**.

A GPIO pin (PA8) toggles every time an ADC conversion completes, generating a **5 kHz square wave** for oscilloscope verification.

---

## 🛠️ Key Specifications

| Feature | Details |
|--------|---------|
| MCU | STM32-F411RET6 (Nucleo-F411RE) |
| ADC | ADC1, Channel 0 (PA0) |
| Trigger Source | TIM2 TRGO (Update Event) |
| Timer Clock | 84 MHz |
| Sample Rate | 5 kHz (100 µs interval) |
| ADC Mode | Single Conversion, Interrupt-Driven |
| Verification | PA8 Toggle Output → 10 kHz Square Wave |

---

## 🔧 Hardware Setup

| Pin | Function |
|-----|----------|
| PA0 | Analog input (ADC1_CH0) |
| PA8 | GPIO output toggle for scope measurement |

Optional: connect a **function generator** to PA0 to capture real waveform sampling.

---

## 📡 Clock Configuration

- System Clock: **84 MHz PLL**
- APB1: **42 MHz TIM2 enabled with ×2 factor → 84 MHz**
- TIM2 configured for **100 µs period**:
  

---

## 🧩 Peripheral Configuration

**ADC**
- Trigger Edge: Rising
- External Trigger Source: `ADC_EXTERNALTRIGCONV_T2_TRGO`
- Resolution: 12-bit
- Discontinuous & continuous conversion: Disabled

**Timer (TIM2)**
- TRGO Output: `TIM_TRGO_UPDATE`

---



---

## 📈 Oscilloscope ADC Timing Verification

PA8 Toggle Output — Expected 10 kHz Square Wave

[Oscilloscope Result]
<br>
(https://github.com/vaishnavgophane/STM32F411-ADC-Timer-Triggered-Sampling-100-s-GPIO-Waveform-Test/blob/main/IMG_20251205_174415512_HDR_AE.jpg)

--- 
## 🧮 Timer  Calculations

This project uses Timer 2 to generate periodic triggers for ADC1 sampling at 10 kHz (every 100 µs).
Timer Clock Source

TIM2 is on APB1 bus

APB1 peripheral clock = 42 MHz

Since APB1 prescaler ≠ 1 → Timer clock doubles:

TIM2 Clock = 42 MHz × 2 = 84 MHz

🔹 Prescaler Calculation (PSC)

We want the timer to increment every 1 µs:

PSC = (Timer Clock / Desired Timer Tick) - 1
PSC = (84 MHz / 1 MHz) - 1
PSC = 84 - 1
PSC = 83


✔ Now the timer counter ticks every:
1 / 1 MHz = 1 µs

🔹 Auto-Reload Register (ARR)

To trigger an event every 100 µs:

Timer Ticks Needed = 100 µs / 1 µs = 100
ARR = 100 - 1 = 99

✔ Timer generates update event 100 µs repeatedly

---

## 📌 Code Snippet (ISR Toggle)
```c
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* hadc)
{
    if (hadc->Instance == ADC1)
    {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_8);
    }
}
