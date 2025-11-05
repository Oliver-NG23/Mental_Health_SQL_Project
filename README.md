# 🧠 Mental Health in the Tech Industry – SQL Analysis

Este proyecto explora el dataset **“Mental Health in the Tech Industry”**, el cual reúne información sobre la salud mental de personas que trabajan en el sector tecnológico.  
El objetivo principal es **comprender los factores que influyen en la salud mental dentro de la industria tecnológica** mediante consultas SQL.

---

## 📊 Descripción del Dataset

El dataset proviene de [Kaggle](https://www.kaggle.com/datasets/anth7310/mental-health-in-the-tech-industry) y contiene respuestas anónimas de profesionales del sector tecnológico sobre distintos aspectos de su salud mental y su entorno laboral.

---

## 🎯 Objetivos del Proyecto

1. **Explorar y comprender la estructura del dataset** para identificar sus variables clave y distribución de los datos.  

2. **Analizar los factores relacionados con la salud mental en el entorno laboral tecnológico** percibido por los empleados.  

3. **Examinar patrones demográficos y geográficos** (como edad, género y país) para detectar diferencias en la percepción, el tratamiento y la conciencia sobre la salud mental.  

4. **Generar insights accionables** para encontrar posibles áreas de mejora en la cultura laboral del sector tecnológico.  

---

## ❓ Preguntas de Análisis SQL

1. ¿Cúantos participantes aplicaron la encuesta durante los años?
```sql
SELECT s.SurveyID, s.Description, COUNT(a.AnswerText) AS TotalAnswers
FROM Answer a
JOIN Survey s ON a.SurveyID = s.SurveyID
GROUP BY s.SurveyID, s.Description
ORDER BY s.SurveyID;
```
**Objetivo:** Saber cuantas personas participaron en la aplicacion de la encuesta a lo largo de los años

2. ¿De que paises provienen los encuestados?
```sql
SELECT 
	CASE WHEN answertext IN ('United States of America','United States') THEN 'United States'
    ELSE answertext END AS pais, COUNT(*) AS cuenta
FROM Answer
WHERE questionid = 3 AND answertext NOT IN( '-1', 'N/A','') AND answertext IS NOT NULL
GROUP BY pais
ORDER BY cuenta DESC;
```
**Objetivo:** Conocer de que países provienen los encuestados
### Nivel 2 – Filtrado y agrupaciones simples

4. ¿Qué porcentaje de encuestados ha recibido tratamiento para su salud mental? 
```sql
SELECT CASE WHEN a.AnswerText = 1 THEN 'YES' ELSE 'NO' END AS Respuesta,
	COUNT(a.AnswerText) AS Total_Respuesta,
    ROUND(COUNT(*) * 100.0 / SUM (COUNT(*)) OVER (),4) AS porcentaje
FROM Answer AS a
JOIN Question AS q
ON a.QuestionID = q.QuestionID
WHERE q.QuestionText LIKE '%Have you ever sought treatment%'
group by a.AnswerText;
```
**Objetivo:** Conocer antecedentes de los empleados y su distribucion

5. ¿Cuál es el salario promedio por género o por país?  
6. ¿Cuántos encuestados indicaron que su empresa tiene una política de salud mental, y cuántos no?

### Nivel 3 – Relaciones e indicadores más complejos
7. ¿Existe relación entre el tamaño de la empresa y la probabilidad de haber recibido tratamiento?  
8. ¿Cómo varía la edad promedio entre quienes han recibido tratamiento y quienes no?  
9. ¿Cuáles son los 5 países con mayor porcentaje de encuestados que reportan haber recibido tratamiento?

### Nivel 4 – Subconsultas y funciones avanzadas
10. Para cada empresa, ¿qué porcentaje de empleados se siente cómodo hablando de salud mental con su supervisor?  
11. ¿Cómo cambia el nivel de estrés promedio según los años trabajados en la empresa (agrupando por rangos)?  
12. Calcula el percentil de salario por país y analiza si los trabajadores con salarios más altos (percentil 90 o superior) tienden a recibir más o menos tratamiento que el resto.

---
