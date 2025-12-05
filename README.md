## 🧑‍💻 Author

**Vaishnav Gophane**  
Embedded Firmware & IoT Developer
<br>
Pune, India.

📫 **Connect:** [Gmail](mr.vaishnavgophane@gmail.com) • [GitHub](https://github.com/vaishnavgophane) • [LinkedIn](https://www.linkedin.com/in/vaishnav-gophane-417686284/)

---
# STM32F411 ADC Timer Triggered Sampling (100µs)

This project demonstrates **precisely timed ADC sampling** using **TIM2 TRGO** as an external trigger source on the **STM32 Nucleo-F411RE**.

A GPIO pin (PA8) toggles every time an ADC conversion completes, generating a **10 kHz square wave** for oscilloscope verification.

---

## 🛠️ Key Specifications

| Feature | Details |
|--------|---------|
| MCU | STM32-F411RET6 (Nucleo-F411RE) |
| ADC | ADC1, Channel 0 (PA0) |
| Trigger Source | TIM2 TRGO (Update Event) |
| Timer Clock | 84 MHz |
| Sample Rate | 10 kHz (100 µs interval) |
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

## 📌 Code Snippet (ISR Toggle)

void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* hadc)
{
    if (hadc->Instance == ADC1)
    {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_8);
    }
}

---

## 📈 Oscilloscope ADC Timing Verification

PA8 Toggle Output — Expected 10 kHz Square Wave

[Oscilloscope Result]()
