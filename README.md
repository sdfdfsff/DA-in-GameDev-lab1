# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Артеменко Антон Сергеевич
- РИ210950
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с работой перцептрона

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления
Ход работы:

- Добавил к пустому объекту скрипт Perceptron
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab4/Screenshot_1.png)

- Заполнил элементы 
1. OR, понадобилось 3 эпохи, 4 вычисляла корректно
```
void Start() {
  Train(4);
  Debug.Log("Test 0 0: " + CalcOutput(0,0));
  Debug.Log("Test 0 1: " + CalcOutput(0,1));
  Debug.Log("Test 1 0: " + CalcOutput(1,0));
  Debug.Log("Test 1 1: " + CalcOutput(1,1));
}
```
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab4/Screenshot_2.png)

2. AND, понадобилось 4 эпохи, 5 вычисляла корректно
```
void Start() {
  Train(5);
  Debug.Log("Test 0 0: " + CalcOutput(0,0));
  Debug.Log("Test 0 1: " + CalcOutput(0,1));
  Debug.Log("Test 1 0: " + CalcOutput(1,0));
  Debug.Log("Test 1 1: " + CalcOutput(1,1));
}
```
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab4/Screenshot_3.png)

3. NAND, понадобилось 5 эпохи, 6 вычисляла корректно
```
void Start() {
  Train(6);
  Debug.Log("Test 0 0: " + CalcOutput(0,0));
  Debug.Log("Test 0 1: " + CalcOutput(0,1));
  Debug.Log("Test 1 0: " + CalcOutput(1,0));
  Debug.Log("Test 1 1: " + CalcOutput(1,1));
}
```
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab4/Screenshot_4.png)

4. NAND, не вычисляла даже после 100.000 эпох так как не смогла научится
```
void Start() {
  Train(100000);
  Debug.Log("Test 0 0: " + CalcOutput(0,0));
  Debug.Log("Test 0 1: " + CalcOutput(0,1));
  Debug.Log("Test 1 0: " + CalcOutput(1,0));
  Debug.Log("Test 1 1: " + CalcOutput(1,1));
}
```
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab4/Screenshot_5.png)

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения
Ход работы:
- благодаря полученным данным, построил график в excel
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab1/Screenshot_6.png)

- Количество эпох зависит от смещения и веса
```
double DotProductBias(double[] v1, double[] v2){
  if (v1 == null || v2 == null) return -1;
  if (v1.Length != v2.Lenght) return -1;
  double d = 0;
  for (int x = 0; x < v1.Lenght; x++){
    d += v1[x]*v2[x];
  }
  d += bias;
  return d;
}

double CalcOutpyt(int i){
  double dp = DotProductBias(weights,ts[i].input);
  if (dp > 0) return(1);
  return (0);
}
```
## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity
Ход работы:
- модель для работы NAND, 
черные - нули
белые - единицы
![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/lab1/Screenshot_7.png)

- скрипт для изменения цвета при столкновении
```
using UnityEngine;

public class ChangeColor : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.GetComponent<Renderer>().material.color == Color.black && this.gameObject.GetComponent<Renderer>().material.color == Color.black)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
        else if (other.gameObject.GetComponent<Renderer>().material.color == Color.black && this.gameObject.GetComponent<Renderer>().material.color == Color.white)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
        else if (other.gameObject.GetComponent<Renderer>().material.color == Color.white && this.gameObject.GetComponent<Renderer>().material.color == Color.black)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
        else {
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
            this.gameObject.GetComponent<Renderer>().material.color = Color.black;
        }
    }
}
```
## Выводы

- Я познакомился с работой перцептрона, который вычисляет различные функции, построил график зависимостей, построил визуальную модель работы перцептрона на сцене Unity
