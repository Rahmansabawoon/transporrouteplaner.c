#include <stdio.h>
#include <math.h>

#define NUM_MAHALLE 3

// Mahalle verileri
typedef struct {
    char name[20];
    int population_density; // Nüfus yoğunluğu
    int transport_infrastructure; // Ulaşım altyapısı
    int cost; // Maliyet
    int environmental_impact; // Çevresel etki
    int social_benefit; // Sosyal fayda
} Mahalle;

// Ağırlıklar
double weights[] = {0.25, 0.20, -0.15, 0.20, 0.20};

// Softmax fonksiyonu
void softmax(double scores[], double probabilities[]) {
    double max_score = scores[0];
    for (int i = 1; i < NUM_MAHALLE; i++) {
        if (scores[i] > max_score) {
            max_score = scores[i];
        }
    }

    double exp_scores[NUM_MAHALLE];
    double sum_exp_scores = 0.0;

    for (int i = 0; i < NUM_MAHALLE; i++) {
        exp_scores[i] = exp(scores[i] - max_score);
        sum_exp_scores += exp_scores[i];
    }

    for (int i = 0; i < NUM_MAHALLE; i++) {
        probabilities[i] = exp_scores[i] / sum_exp_scores;
    }
}

int main() {
    Mahalle mahalleler[NUM_MAHALLE] = {
        {"Mahalle A", 300, 8, 500000, 7, 9},
        {"Mahalle B", 450, 6, 600000, 5, 8},
        {"Mahalle C", 200, 9, 400000, 6, 7}
    };

    double scores[NUM_MAHALLE];
    double probabilities[NUM_MAHALLE];

    // Puan hesaplama
    for (int i = 0; i < NUM_MAHALLE; i++) {
        scores[i] = (weights[0] * mahalleler[i].population_density) +
                     (weights[1] * mahalleler[i].transport_infrastructure) +
                     (weights[2] * mahalleler[i].cost) +
                     (weights[3] * mahalleler[i].environmental_impact) +
                     (weights[4] * mahalleler[i].social_benefit);
    }

    // Softmax uygulama
    softmax(scores, probabilities);

    // Sonuçları yazdırma
    printf("Mahalleler için olasılıklar:\n");
    for (int i = 0; i < NUM_MAHALLE; i++) {
        printf("%s: %.4f\n", mahalleler[i].name, probabilities[i]);
    }

    return 0;
}
