"""
Simulación y análisis de señales utilizando la Transformada de Fourier
Autor: Mario
Descripción:
Este programa genera señales básicas (senoidal, pulso rectangular y escalón),
calcula su Transformada de Fourier usando FFT y grafica la señal en el dominio
del tiempo y frecuencia (magnitud y fase).
"""

import numpy as np
import matplotlib.pyplot as plt

# ==============================
# 1. PARÁMETROS DE MUESTREO
# ==============================

fs = 1000  # Frecuencia de muestreo (Hz)
T = 2      # Duración total (segundos)
t = np.linspace(-T/2, T/2, fs)

# ==============================
# 2. DEFINICIÓN DE SEÑALES
# ==============================

# Señal senoidal
f = 5  # frecuencia en Hz
senal_senoidal = np.sin(2 * np.pi * f * t)

# Pulso rectangular
senal_rectangular = np.where(np.abs(t) < 0.2, 1, 0)

# Función escalón
senal_escalon = np.where(t >= 0, 1, 0)

# ==============================
# 3. FUNCIÓN PARA FFT
# ==============================

def analizar_fft(senal, nombre):
    """
    Calcula y grafica la FFT de una señal
    """
    fft_senal = np.fft.fft(senal)
    frecuencias = np.fft.fftfreq(len(t), d=1/fs)

    magnitud = np.abs(fft_senal)
    fase = np.angle(fft_senal)

    # Dominio del tiempo
    plt.figure()
    plt.plot(t, senal)
    plt.title(f"{nombre} - Dominio del Tiempo")
    plt.xlabel("Tiempo (s)")
    plt.ylabel("Amplitud")
    plt.grid()
    plt.show()

    # Magnitud
    plt.figure()
    plt.plot(frecuencias, magnitud)
    plt.title(f"{nombre} - Magnitud del Espectro")
    plt.xlabel("Frecuencia (Hz)")
    plt.ylabel("Magnitud")
    plt.grid()
    plt.show()

    # Fase
    plt.figure()
    plt.plot(frecuencias, fase)
    plt.title(f"{nombre} - Fase del Espectro")
    plt.xlabel("Frecuencia (Hz)")
    plt.ylabel("Fase (rad)")
    plt.grid()
    plt.show()

# ==============================
# 4. ANÁLISIS DE CADA SEÑAL
# ==============================

analizar_fft(senal_senoidal, "Señal Senoidal")
analizar_fft(senal_rectangular, "Pulso Rectangular")
analizar_fft(senal_escalon, "Función Escalón")

# ==============================
# 5. VERIFICACIÓN DE PROPIEDADES
# ==============================

# Propiedad de linealidad
senal_suma = senal_senoidal + senal_rectangular
analizar_fft(senal_suma, "Suma de Señales (Linealidad)")

# Desplazamiento en el tiempo
desplazamiento = 0.3
senal_desplazada = np.sin(2 * np.pi * f * (t - desplazamiento))
analizar_fft(senal_desplazada, "Señal Desplazada")

# Escalamiento en frecuencia
senal_escalada = np.sin(2 * np.pi * (2*f) * t)
analizar_fft(senal_escalada, "Señal con Frecuencia Escalada")

print("Análisis completado correctamente.")



