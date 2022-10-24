# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
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
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity. При выполнении задания можно использовать видео-материалы и исходные данные, предоставленные преподавателями курса.
Ход работы:

-	Создайте новый пустой 3D проект на Unity.
-	Скачайте папку с ML агентом. Вы найдете ее в облаке с исходными файлами к лабораторной работе – ml-agents-release_19.
-	В созданный проект добавьте ML Agent, выбрав Window - Package Manager - Add Package from disk. Последовательно добавьте .json – файлы

![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/main/lab1/Screenshot_1.png)

-	Если все сделано правильно, то во вкладке с компонентами (Components) внутри Unity вы увидите строку ML Agent.

![Image alt](https://github.com/sdfdfsff/DA-in-GameDev-lab1/blob/main/lab1/Screenshot_2.png)


-	Далее запускаем Anaconda Prompt для возможности запуска команд через консоль.
-	Далее пишем серию команд для создания и активации нового ML-агента, а также для скачивания необходимых библиотек


-	Создайте на сцене плоскость, куб и сферу так, как показано на рисунке ниже. Создайте простой C# скрипт-файл и подключите его к сфере
-	В скрипт-файл RollerAgent.cs добавьте код, опубликованный в материалах лабораторных работ – по ссылке.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```

-	Объекту «сфера» добавить компоненты Decision Requester, Behavior Parameters и настройте их так, как показано на рисунке ниже

-	В корень проекта добавьте файл конфигурации нейронной сети, доступный в папке с файлами проекта по ссылке.

-	Запустите работу ml-агента
-	Вернитесь в проект Unity, запустите сцену, проверьте работу ML-Agent’a.

-	Сделайте 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустите симуляцию сцены и наблюдайте за результатом обучения модели.

-	После завершения обучения проверьте работу модели.
- Сделайте выводы
При увеличении количества копий, модель обучается быстрее. По итогу обучения модели, шар может успешно достичь куба.

## Задание 2
### Подробно опишите каждую строку файла конфигурации нейронной сети, доступного в папке с файлами проекта по ссылке. Самостоятельно найдите информацию о компонентах Decision Requester, Behavior Parameters, добавленных сфере.

```yaml
behaviors: # описание конфигурации  
   RollerBall: # название конфигурации  
    trainer_type: ppo # Тип тренажера для использования  
    hyperparameters: # Гиперпараметры для ppo  
      batch_size: 10 # Количество опытов в каждой итерации градиентного спуска  
      buffer_size: 100 # Количество опытов, которые необходимо собрать перед обновлением модели политики. 
      # Соответствует тому, сколько опыта должно быть собрано, прежде чем мы будем изучать или обновлять модель  
      learning_rate: 3.0e-4 # Начальная скорость обучения для градиентного спуска  
      beta: 5.0e-4 # Сила энтропийной регуляризации, которая делает политику «более случайной»  
      epsilon: 0.2 # Влияет на то, насколько быстро политика может развиваться во время обучения  
      lambd: 0.99 # Параметр регуляризации (лямбда), используемый при расчете обобщенной оценки преимущества  
      num_epoch: 3 # Количество проходов через буфер опыта при оптимизации градиентного спуска  
      learning_rate_schedule: linear # Определяет, как скорость обучения изменяется с течением времени  
    network_settings:  
      normalize: false # Применяется ли нормализация к входным данным векторного наблюдения  
      hidden_units: 128 # Количество юнитов в скрытых слоях нейронной сети  
      num_layers: 2 # Количество скрытых слоев в нейронной сети  
    reward_signals:  
      extrinsic:   
        gamma: 0.99 # Коэффициент дисконтирования для будущих вознаграждений, поступающих из окружающей среды  
        strength: 1.0 # Фактор, на который умножается вознаграждение, данное окружающей средой  
    max_steps: 500000 # общее количество шагов (т. е. собранных наблюдений и предпринятых действий),
    # которые необходимо выполнить в среде (или во всех средах при параллельном использовании нескольких) перед завершением процесса обучения  
    time_horizon: 64 # Сколько шагов опыта нужно собрать для каждого агента, прежде чем добавить его в буфер опыта  
    summary_freq: 10000 # Количество опытов, которые необходимо собрать перед созданием и отображением статистики обучения  
```

## Задание 3
### Доработайте сцену и обучите ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и впервом задании, случайно изменять кооринаты на плоскости. 

- Изменим код RollerAgent.cs так, чтобы сфера следовала сначала к одному кубу, затем к другому
```c#
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    [SerializeField] private Transform firstTarget;
    [SerializeField] private Transform secondTarget;
    private Transform currentTarget;
    Rigidbody rBody;

    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public override void OnEpisodeBegin()
    {
        if (transform.localPosition.y < 0)
        {
            rBody.angularVelocity = Vector3.zero;
            rBody.velocity = Vector3.zero;
            transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        firstTarget.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 4);
        secondTarget.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * -4);

        if (Vector3.Distance(transform.localPosition, firstTarget.localPosition) >
            Vector3.Distance(transform.localPosition, secondTarget.localPosition))
            (firstTarget, secondTarget) = (secondTarget, firstTarget);

        currentTarget = firstTarget;
    }

    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(currentTarget.localPosition);
        sensor.AddObservation(transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }

    public float forceMultiplier = 10;

    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        var controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        var distanceToTarget = Vector3.Distance(transform.localPosition, currentTarget.localPosition);

        if (distanceToTarget < 1.42f && currentTarget != secondTarget)
            currentTarget = secondTarget;
        else if (distanceToTarget < 1.42f && currentTarget == secondTarget)
        {
            SetReward(1.0f);
            EndEpisode();
        }

        if (transform.localPosition.y < 0)
            EndEpisode();
    }
}
```
- Зададим параметры RollerAgent в Unity
- Переобучим модель
- Проверим результат

## Выводы

Баланс - это попытка сделать игру более цельной и играбельной, чтобы она не была слишком просто или слишком сложной. Баланс это узкий диапазон фана, в который нужно попасть разработчику чтобы его достичь. 

Я думаю, что машинное обучение может помочь разработчикам скорректировать гейплей для достижения баланса. К примеру, сделать определенные участки игры достаточно сложными, но при этом вполне доступными для прохождения игроку.  

В этой лабораторной работе мы научились создавать и обучать MlAgent, который потом позволит шару успешно добираться до своей цели.
