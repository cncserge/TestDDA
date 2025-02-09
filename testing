#include <stdint.h>
#include <stdio.h>
#include <stdbool.h>
#include <math.h>

#define STEPS_PER_MM 100  // Количество шагов на мм (примерное значение)

// Структура для хранения параметров шагового двигателя
typedef struct {
    int32_t current;    // Текущее положение
    int32_t target;     // Целевое положение
    int32_t delta;      // Дельта (разница между текущим и целевым положением)
    int32_t error;      // Ошибка (используется в алгоритме DDA)
    bool direction;     // Направление движения (true - вперед, false - назад)
} Stepper;

// Функция инициализации двигателя
void initStepper(Stepper *stepper, int32_t target) {
    stepper->target = target * STEPS_PER_MM; // Перевод целевого значения в шаги
    stepper->current = 0; // Начальное положение
    stepper->delta = abs(stepper->target); // Абсолютное значение дельты
    stepper->error = 0; // Ошибка обнуляется
    stepper->direction = stepper->target >= 0; // Определение направления движения
}

// Функция пошагового управления двигателями
void ddaStep(Stepper *x, Stepper *y, Stepper *z) {
    // Определение наибольшего значения дельты среди двигателей
    int32_t max_delta = fmax(fmax(x->delta, y->delta), z->delta);
    
    // Основной цикл управления шагами двигателей
    for (int32_t i = 0; i < max_delta; i++) {
        // Управление двигателем X
        if (x->error >= max_delta) {
            x->current += x->direction ? 1 : -1; // Совершение шага в нужном направлении
            x->error -= max_delta; // Коррекция ошибки
            printf("Step X\n"); // Вывод информации о шаге
        }
        x->error += x->delta; // Накопление ошибки

        // Управление двигателем Y
        if (y->error >= max_delta) {
            y->current += y->direction ? 1 : -1;
            y->error -= max_delta;
            printf("Step Y\n");
        }
        y->error += y->delta;

        // Управление двигателем Z
        if (z->error >= max_delta) {
            z->current += z->direction ? 1 : -1;
            z->error -= max_delta;
            printf("Step Z\n");
        }
        z->error += z->delta;
    }
}

// Главная функция программы
int main() {
    Stepper x, y, z; // Создание объектов шаговых двигателей
    initStepper(&x, 50);  // Инициализация двигателя X (50 мм)
    initStepper(&y, 30);  // Инициализация двигателя Y (30 мм)
    initStepper(&z, 20);  // Инициализация двигателя Z (20 мм)
    
    ddaStep(&x, &y, &z); // Запуск алгоритма DDA
    
    return 0; // Завершение программы
}
