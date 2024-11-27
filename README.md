# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР
Отчет по лабораторной работе #4 выполнила:
- Миногина Дарья Алексеевна
- РИ-230932
  
Отметка о выполнении заданий:

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

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- в проекте Unity реализовать перцептрон, который умеет производить вычисления:
OR | дать комментарии о корректности работы
AND | дать комментарии о корректности работы
NAND | дать комментарии о корректности работы
XOR | дать комментарии о корректности работы
- Задание 2.
- Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.
- Задание 3.
- Построить визуальную модель работы перцептрона на сцене Unity.
- Выводы.
- ✨Magic✨

## Цель работы
Изучить и реализовать принципы работы перцептрона, базовой модели искусственного нейрона, на платформе Unity.

## Задание 1
###  в проекте Unity реализовать перцептрон, который умеет производить вычисления:
- OR | дать комментарии о корректности работы

Для начала создадим проект, в котором будет GameObject на который мы накинем скрипт перцептрона и где будем менять значения 
![UnityPerceptron](https://github.com/MidoriKsai/Homework4/blob/main/PerceptronUnity.png)

Я везде брала 10 эпох, что много, можно было остановиться на 6-7 эпохах.

Для OR понадобилось примерно 4 эпохи, чтобы исправить ошибки. 
![OR](https://github.com/MidoriKsai/Homework4/blob/main/OR.png)


- AND | дать комментарии о корректности работы
  
Для AND понадобилось примерно 6 эпох, чтобы исправить ошибки.
![AND](https://github.com/MidoriKsai/Homework4/blob/main/AND.png)


- NAND | дать комментарии о корректности работы

Для NAND понадобилось примерно 4 эпохи, чтобы исправить ошибки.
![NAND](https://github.com/MidoriKsai/Homework4/blob/main/NAND.png)

- XOR | дать комментарии о корректности работы

XOR не является линейно разделимой функцией. Перцептрон не смог корректно обучиться и выполнить эту операцию, что можно продемонстрировать в Unity. Первые значения TotalError варьируются, но после 3-4 эпох становятся равны 4 и уже не меняются.
![XOR](https://github.com/MidoriKsai/Homework4/blob/main/XOR.png)


## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.

У меня получились следующие результаты: https://docs.google.com/spreadsheets/d/16u5VYNN4rtL8IFkjwBF_Q1-vaZFuLSgz3QGfbrxCafE/edit?usp=sharing
![GoogleSheetsPerceptron](https://github.com/MidoriKsai/Homework4/blob/main/GoogleSheetsPerceptron.png)

## Задание 3
### Визуализировать данные из google-таблицы, и с помощью Python передать в проект Unity. В Python данные также должны быть визуализированы.

Код для вывода таблицы из GoogleSheets с помощью Python
![PythonDP](https://github.com/MidoriKsai/Homework3/blob/main/PythonDP.png)

Код для передачи данных в Unity
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using Newtonsoft.Json.Linq;

public class EnemyDragon : MonoBehaviour
{
  public GameObject dragonEggPrefab;
  public float speed = 1;
  public float timeBetweenEggDrops = 1f;
  public float leftRightDistance = 10f;
  public float chanceDirection = 0.1f;
  public int lvlNumber = 0;

  private string sheetId = "1wS8rGyOEgJLOPOv45qXWkdj3O-NInPqLVthZDWIjTtA";
  private string apiKey = "AIzaSyA4ZArvrYjVrdmsjiLnKJJH4r25PzAYc7w";

  void Start()
  {
      StartCoroutine(GetDataFromSheet());
      Invoke("DropEgg", 2f);
  }

  void DropEgg(){
      Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
      GameObject egg = Instantiate<GameObject>(dragonEggPrefab);
      egg.transform.position = transform.position + myVector;
      Invoke("DropEgg", timeBetweenEggDrops);
  }

  void Update()
  {
      Vector3 pos = transform.position;
      pos.x += speed * Time.deltaTime;
      transform.position = pos;

      if (pos.x < -leftRightDistance){
          speed = Mathf.Abs(speed);
      }
      else if (pos.x > leftRightDistance){
          speed = -Mathf.Abs(speed);
      }
  }

  private void FixedUpdate() {
      if (Random.value < chanceDirection){
          speed *= -1;
      }
  }

   IEnumerator GetDataFromSheet()
  {
      Debug.Log("Here");
      var charList = new List<string> {"B","C","D","E","F","G","H","I","J","K"};
      var charToPut = charList[lvlNumber-1];
      Debug.Log(charToPut);
      string url = $"https://sheets.googleapis.com/v4/spreadsheets/{sheetId}/values/Лист1!{charToPut}2:{charToPut}5?key={apiKey}";
      Debug.Log(url);

      using (UnityWebRequest request = UnityWebRequest.Get(url))
      {
          yield return request.SendWebRequest();

          if (request.result == UnityWebRequest.Result.Success)
          {
                              
              JObject json = JObject.Parse(request.downloadHandler.text);
              JArray values = (JArray)json["values"];
              Debug.Log(values);

              if (values != null && values.Count > 0)
              {
                  
                  speed = float.Parse((string)values[0][0]);
                  timeBetweenEggDrops = float.Parse((string)values[3][0]); 
                  leftRightDistance = float.Parse((string)values[1][0]); 
                  chanceDirection = float.Parse((string)values[2][0]); 
              }
          }
          else
          {
              Debug.LogError($"Error fetching data: {request.error}");
          }
      }
   }
} 
```

## Выводы

Работа над балансировкой параметров игры в Dragon Picker показала, как ключевые переменные, такие как скорость движения, вероятность смены направления и частота падения предметов, напрямую влияют на игровой процесс. Эти параметры задают темп и сложность игры, и их корректная настройка позволяет создавать постепенно усложняющийся игровой опыт.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
