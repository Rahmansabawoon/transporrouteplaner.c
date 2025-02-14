#include <stdio.h>
#include <math.h>

#define MAHALLE_SAYISI 3

// Softmax fonksiyonu
void softmax(double *input, double *output, int n) {
    double max_val = input[0];
    for (int i = 1; i < n; i++) {
        if (input[i] > max_val) {
            max_val = input[i];
        }
    }

    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        output[i] = exp(input[i] - max_val);
        sum += output[i];
    }

    for (int i = 0; i < n; i++) {
        output[i] /= sum; // Normalize et
    }
}

int main() {
    // Mahalle verileri
    char *mahalleler[MAHALLE_SAYISI] = {"Mahalle A", "Mahalle B", "Mahalle C"};
    double nufus_yogunlugu[MAHALLE_SAYISI] = {1000, 1500, 800}; // kişi/km²
    double ulasim_altyapisi[MAHALLE_SAYISI] = {3, 5, 2}; // mevcut hat sayısı
    double maliyet[MAHALLE_SAYISI] = {20000, 30000, 15000}; // TL
    double cevresel_etki[MAHALLE_SAYISI] = {2, 3, 4}; // düşükten yükseğe
    double sosyal_fayda[MAHALLE_SAYISI] = {4, 5, 3}; // düşükten yükseğe

    double nufus_weights[MAHALLE_SAYISI];
    double ulasim_weights[MAHALLE_SAYISI];
    double maliyet_weights[MAHALLE_SAYISI];
    double cevresel_weights[MAHALLE_SAYISI];
    double sosyal_weights[MAHALLE_SAYISI];
    double toplam_weights[MAHALLE_SAYISI] = {0};

    // Ağırlıkları hesapla
    softmax(nufus_yogunlugu, nufus_weights, MAHALLE_SAYISI);
    softmax(ulasim_altyapisi, ulasim_weights, MAHALLE_SAYISI);
    softmax(maliyet, maliyet_weights, MAHALLE_SAYISI); // Düşük maliyet yüksek değer
    softmax(cevresel_etki, cevresel_weights, MAHALLE_SAYISI); // Düşük etki yüksek değer
    softmax(sosyal_fayda, sosyal_weights, MAHALLE_SAYISI);

    // Toplam ağırlıkları hesapla
    for (int i = 0; i < MAHALLE_SAYISI; i++) {
        toplam_weights[i] = (nufus_weights[i] + ulasim_weights[i] + 
                             (1 - maliyet_weights[i]) + (1 - cevresel_weights[i]) + 
                             sosyal_weights[i]) / 5.0; // Normalizasyon
    }

    // En uygun güzergahı belirle
    int en_uygun_index = 0;
    for (int i = 1; i < MAHALLE_SAYISI; i++) {
        if (toplam_weights[i] > toplam_weights[en_uygun_index]) {
            en_uygun_index = i;
        }
    }

    printf("En uygun güzergah: %s\n", mahalleler[en_uygun_index]);
    
    return 0;
}

