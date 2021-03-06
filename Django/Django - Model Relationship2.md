# π±Django - Model Relationship 2

## λ³μ μ§λ£ κΈ°λ‘ μμ€ν (M:N κ΄κ³)

- 1:N λͺ¨λΈ κ΄κ³ μ€μ 

```python
# hospitals/models.py

class Doctor(model.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}λ² μμ¬ {self.name}'
    
class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}λ² νμ {self.name}'
```

- μμ¬ 2λͺκ³Ό νμ 2λͺ μμ±

```shell
doctor1 = Doctor.objects.create(name='justin')
doctor2 = Doctor.objects.create(name='eric')

patient1 = Patient.objects.create(name='tony', doctor=doctor1)
patient2 = Patient.objects.create(name='harry', doctor=doctor2)
```

| hospitals_doctor |        |      | hospitals_patient |       |           |
| :--------------: | :----: | ---- | :---------------: | :---: | :-------: |
|        id        |  name  |      |        id         | name  | doctor_id |
|        1         | justin |      |         1         | tony  |     1     |
|        2         |  eric  |      |         2         | harry |     2     |

## 1:Nμ νκ³

- 1λ² νμ(tony)κ° 1λ² μμ¬μ μ§λ£λ₯Ό λ§μΉκ³ , 2λ² μμ¬μκ²λ λ°©λ¬Ένλ €κ³  νλ€λ©΄, μλ‘μ΄ μμ½μ μμ±ν΄μΌ νλ€.
- κΈ°μ‘΄μ μμ½μ μ μ§ν μνλ‘ μλ‘μ΄ μμ½μ μμ±νκ² λκ³ , μλ‘ μμ±ν 3λ² νμ(tony)λ 1λ² νμ(tony)μ λ€λ₯΄λ€!
- μ΄λ€ νμκ° ν λ²μ λ μμ¬μκ² μ§λ£λ₯Ό λ°λ κ²½μ°λ μκ³ , μ΄λ₯Ό μ μ©νλ €κ³  νλμ μΈλ ν€μ 2κ°μ μμ¬ λ°μ΄ν°λ₯Ό λ£μ μκ°μ ν  μ μμ§λ§ μλ¬κ° λ°μ 

> 1:NμΌλ‘λ μλ‘μ΄ μμ½μ μμ±νλ κ²μ΄ λΆκ°λ₯νκΈ° λλ¬Έμ μλ‘μ΄ κ°μ²΄λ₯Ό μμ±ν΄μΌ νλ€. λν μ¬λ¬ μμ¬μκ² μ§λ£ λ°μ κΈ°λ‘μ νμ ν λͺμ μ μ₯ν  μ μλ€!

### μ€κ° λͺ¨λΈ

> μ€κ° λͺ¨λΈ νΉμ μ€κ° νμ΄λΈμ μμ±

```python
# hospitals/models.py

from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² μμ¬ {self.name}'

    
class Patient(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² νμ {self.name}'

# μ€κ°λͺ¨λΈ μμ±
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}λ² μμ¬μ {self.patient_id}λ² νμ'

```

- μ€κ° λͺ¨λΈκ³Όμ λͺ¨λΈ κ΄κ³ νμΈ
- ν λͺμ μμ¬κ° μ¬λ¬ κ°μ μ§λ£ μμ½μ κ°μ§ μ μμ, λ§μ°¬κ°μ§λ‘ νλͺμ νμλ μ¬λ¬ μ§λ£ μμ½μ κ°μ§ μ μμ

![image-20220418195602420](Django%20-%20Model%20Relationship2.assets/image-20220418195602420.png)

- μμ¬ 1λͺκ³Ό νμ 1λͺ μμ± λ° μμ½ μμ±

```bash
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')

Reservation.objects.create(doctor=doctor1, patient=patient1)
```

- μμ¬1μ μμ½ νμ μ‘°ν

```bash
doctor1.reservation_set.all()
<QuerySet [<Reservation: 1λ² μμ¬μ 1λ² νμ>]>
```

- νμμ λ΄λΉ μμ¬ μ‘°ν

```bash
patient1.reservation_set.all()
<QuerySet [<Reservation: 1λ² μμ¬μ 1λ² νμ>]>
```

- νμ 1λͺ μΆκ° μμ± λ° 1λ² μμ¬μκ² μμ½ μμ±

```bash
patient2 = Patient.objects.create(name='harry')
Reservation.objects.create(doctor=doctor1, patient=patient2)

doctor1.reservation_set.all()
<QuerySet [<Reservation: 1λ² μμ¬μ 1λ² νμ>, <Reservation: 1λ² μμ¬μ 2λ² νμ>]>
```



### ManyToManyField

> M:N(many-to-many) κ΄κ³ μ€μ  μ μ¬μ©νλ λͺ¨λΈ νλλ‘ νλμ νμ μμΉ μΈμ(M:N κ΄κ³λ‘ μ€μ ν  λͺ¨λΈ ν΄λμ€)κ° νμ!

- κΈ°μ‘΄μ μ€κ° λͺ¨λΈμ μ­μ νκ³  `ManyToManyField`μ μμ±νλ€. νλ μμ± μμΉλ Doctor λλ Patient λͺ¨λ μμ± κ°λ₯νλ€!

```python
# hospitals/models.py

from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² μμ¬ {self.name}'

class Patient(models.Model):
    # ManyToManyField μμ±
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² νμ {self.name}'
```

- ManyToManyFieldκ° μ€κ° νμ΄λΈμ μμ±ν΄μ€λ€!

![image-20220418202808581](Django%20-%20Model%20Relationship2.assets/image-20220418202808581.png)

- μμ¬ 1λͺκ³Ό νμ 2λͺ μμ±

```BASH
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')
patient2 = Patient.objects.create(name='harry')
```

1. ```bash
   # νμ 1μ΄ doctor1μκ² μμ½ (μ°Έμ‘°)
   patient1.doctors.add(doctor1)
   
   patient1.doctors.all() # νμ 1μ΄ μμ½ν μμ¬ λͺ©λ‘
   <QuerySet [<Doctor: 1λ² μμ¬ justin>]>
   
   doctor1.patient_set.all() # μμ¬ 1μ΄ μμ½ν νμ λͺ©λ‘
   <QuerySet [<Patient: 1λ² νμ tony>]>
   ```

2. ```bash
   # μμ¬ 1μ΄ νμ 2λ₯Ό μμ½ (μ­μ°Έμ‘°)
   doctor1.patient_set.add(patient2)
   
   doctor1.patient_set.all() # μμ¬ 1μ΄ μμ½ν νμ λͺ©λ‘
   <QuerySet [<Patient: 1λ² νμ tony>, <Patient: 2λ² νμ harry>]>
   
   patient2.doctors.all() # νμ 2κ° μμ½ν μμ¬ λͺ©λ‘
   <QuerySet [<Doctor: 1λ² μμ¬ justin>]>
   
   patient1.doctors.all() # νμ 1μ΄ μμ½ν μμ¬ λͺ©λ‘
   <QuerySet [<Doctor: 1λ² μμ¬ justin>]>
   ```

3. μ€κ° νμ΄λΈ νμ΄ν° (`hospitals_patient_doctors`) νμΈ

   ![image-20220418203711030](Django%20-%20Model%20Relationship2.assets/image-20220418203711030.png)

4. ```bash
   # νμ 2κ° μμ¬ 1μ μ§λ£ μμ½ μ·¨μ (μ°Έμ‘°)
   patient2.doctors.remove(doctor1)
   
   patient2.doctors.all() # νμ 2κ° μμ½ν μμ¬ λͺ©λ‘
   <QuerySet []>
   
   doctor1.patient_set.all() # μμ¬ 1μ΄ μμ½ν νμ λͺ©λ‘
   <QuerySet [<Patient: 1λ² νμ tony>>
   ```

5. ```bash
   # μμ¬ 1μ΄ νμ 1μ μ§λ£ μμ½ μ·¨μ (μ­μ°Έμ‘°)
   doctor1.patient_set.remove(patient1)
   
   doctor1.patient_set.all() # μμ¬ 1μ΄ μμ½ν νμ λͺ©λ‘
   <QuerySet []>
   
   patient1.doctors.all() # νμ 1μ΄ μμ½ν μμ¬ λͺ©λ‘
   <QuerySet []>
   ```

### related_name

- target model(κ΄κ³ νλλ₯Ό κ°μ§μ§ μμ λͺ¨λΈ)μ΄ source model(κ΄κ³ νλλ₯Ό κ°μ§ λͺ¨λΈ)μ μ°Έμ‘°ν  λ μ¬μ©ν  managerμ μ΄λ¦μ μ€μ 
- μ¦, μ­μ°Έμ‘° μμ μ¬μ©νλ managerμ μ΄λ¦μ μ€μ 
- ForeignKeyμ `related_name`κ³Ό λμΌ

```python
class Patient(models.Model):
    # ManyToManyField - related_name μμ±
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()
```

- doctor1μ μμ½ νμ λͺ©λ‘ νμΈ ν΄λ³΄κΈ° (μ­μ°Έμ‘°)
- `related_name` μ€μ  ν κΈ°μ‘΄μ `_set manager`λ λ μ΄μ μ¬μ©ν  μ μμ

![image-20220418210717477](Django%20-%20Model%20Relationship2.assets/image-20220418210717477.png)



### Djangoμμ μ€κ° λͺ¨λΈ(νμ΄λΈ)

- djangoλ `ManyToManyField`λ₯Ό ν΅ν΄ μ€κ° νμ΄λΈμ μλμΌλ‘ μμ±νλ€. 
- κ·Έλ λ€λ©΄ μ€κ° νμ΄λΈμ μ§μ  μμ±νλ κ²½μ°λ μμκΉ?
  - μ€κ° νμ΄λΈμ μλμΌλ‘ μ§μ νλ €λ κ²½μ° `through` μ΅μμ μ¬μ©νμ¬, μ€κ° νμ΄λΈμ λνλ΄λ Django λͺ¨λΈμ μ§μ ν  μ μλ€!
  - κ°μ₯ μΌλ°μ μΈ μ©λλ μ€κ° νμ΄λΈμ μΆκ° λ°μ΄ν°λ₯Ό μ¬μ©ν΄ λ€λλ€ κ΄κ³λ‘ μ°κ²°νλ €λ κ²½μ°μ μ¬μ©νλ€.

### μμ½

- μ€μ  Doctorμ Patient νμ΄λΈμ΄ λ³νλ κ²μ μμ
- 1:N κ΄κ³λ μμ ν μ’μμ κ΄κ³μ΄μ§λ§, M:N κ΄κ³λ μμ¬μκ² μ§μ°°λ°λ νμ, νμλ₯Ό μ§μ°°νλ μμ¬μ λκ°μ§ ννλ‘ λͺ¨λ ννμ΄ κ°λ₯ν κ² 



## ManyToManyField

> M:N κ΄κ³ μ€μ  μ μ¬μ©νλ λͺ¨λΈ νλλ‘ νλμ νμ μμΉμΈμ(M:N κ΄κ³λ‘ μ€μ ν  λͺ¨λΈ ν΄λμ€)κ° νμ!
>
> λͺ¨λΈ νλμ RelatedManager(1:N λλ M:N κ΄λ ¨ μ»¨νμ€νΈμμ μ¬μ©λλ manager)λ₯Ό μ¬μ©νμ¬ κ΄λ ¨ κ°μ²΄λ₯Ό μΆκ°, μ κ±° λλ λ§λ€ μ μμ
>
> - add(), remove(), create(), clear()...

### Arguments

1. `related_name`

   - target model (κ΄κ³ νλλ₯Ό κ°μ§μ§ μμ λͺ¨λΈ)μ΄ source model (κ΄κ³ νλλ₯Ό κ°μ§ λͺ¨λΈ)μ μ°Έμ‘°ν  λ(μ­μ°Έμ‘° μ) μ¬μ©ν  managerμ μ΄λ¦μ μ€μ 
   - ForeignKeyμ related_nameκ³Ό λμΌ

2. `through`

   - μ€κ° νμ΄λΈμ μ§μ  μμ±νλ κ²½μ°, through μ΅μμ μ¬μ©νμ¬ μ€κ° νμ΄λΈμ λνλ΄λ Django λͺ¨λΈμ μ§μ ν  μ μμ
   - μΌλ°μ μΌλ‘ μ€κ° νμ΄λΈμ μΆκ° λ°μ΄ν°λ₯Ό μ¬μ©νλ λ€λλ€ κ΄κ³μ μ°κ²°νλ €λ κ²½μ° (extra data with a many-to-many relationship)μ μ£Όλ‘ μ¬μ©λ¨

3. `symmetrical`

   - ManyToManyFieldκ° λμΌν λͺ¨λΈ(on self)μ κ°λ¦¬ν€λ μ μμμλ§ μ¬μ©
   - symmetrical=True(κΈ°λ³Έκ°)μΌ κ²½μ° Djangoλ person_set λ§€λμ λ₯Ό μΆκ°νμ§ μμ
   - source λͺ¨λΈμ μΈμ€ν΄μ€κ° target λͺ¨λΈμ μΈμ€ν΄μ€λ₯Ό μ°Έμ‘°νλ©΄, target λͺ¨λΈμ μΈμ€ν΄μ€λ source λͺ¨λΈ μΈμ€ν΄μ€λ₯Ό μλμΌλ‘ μ°Έμ‘°νλλ‘ ν¨

   > κ·ΈλκΉ μ΄κ² λ¬΄μ¨ μλ¦¬λλ©΄, μ΄λκΉμ§λ M:Nμ κ΄κ³κ° μλ‘ λ€λ₯Έ νμ΄λΈμμ μ΄λ£¨μ΄μ‘λ€. κ·Έλ°λ° νκ°μ νμ΄λΈ μμμ μλ‘ λ€λ₯Έ λ°μ΄ν° λΌλ¦¬ M:Nμ κ΄κ³λ₯Ό κ°μ§ μ μλ€. μλ₯Όλ€μ΄ snsμ νλ‘μ° κΈ°λ₯μ μκ°ν΄λ³΄λ©΄ μ μ λΌλ¦¬ μλ‘ νλ‘μ°λ₯Ό νλ©΄μ M:Nμ κ΄κ³λ₯Ό κ°μ§ μ μλλ°, μ΄λ ManyToManyField νλλ₯Ό μ¬μ©νλ€λ©΄ λͺ¨λΈμ selfλΌκ³  μλ ₯ν΄μΌ νκ³  μ΄λ κΈ°λ³Έ κ°μ΄ `symmetrical=True` μ΄λ€.  μ΄ μ΅μμ Trueλ‘ μ€λ€λ©΄ μ΄λ€μΌμ΄ μκΈ°λ, A - > Bλ₯Ό νλ‘μ° νμ λ μλμΌλ‘ B -> Aλ₯Ό νλ‘μ°κ² νκ² λλ€. λ°λΌμ λμΉ­μ μνμ§ μλ κ²½μ° Falseλ‘ μ£Όμ! 

   ```python
   #models.py
   from django.db import models
   
   class Person(models.Model):
       friends = models.ManyToManyField('self')
       # friends = models.ManyToManyField('self', symmetrical=False)
   ```

   

### Related Manager

- 1:N λλ  M:N κ΄λ ¨ μΌνμ€νΈμμ μ¬μ©λλ λ§€λμ 

- κ°μ μ΄λ¦μ λ©μλμ¬λ κ° κ΄κ³(1:N, M:N)μ λ°λΌ λ€λ₯΄κ² μ¬μ© λ° λμ

  - 1:N κ΄κ³μμλ target λͺ¨λΈ μΈμ€ν΄μ€λ§ μ¬μ© κ°λ₯
  - M:N κ΄κ³μμλ κ΄λ ¨λ λ κ°μ²΄μμ λͺ¨λ μ¬μ© κ°λ₯

- λ©μλ μ’λ₯

  - `add()`

    - "μ§μ λ κ°μ²΄λ₯Ό κ΄λ ¨ κ°μ²΄ μ§ν©μ μΆκ°"
    - μ΄λ―Έ μ‘΄μ¬νλ κ΄κ³μ μ¬μ©νλ©΄ κ΄κ³κ° λ³΅μ λμ§ μμ
    - λͺ¨λΈ μΈμ€ν΄μ€, νλ κ°(PK)μ μΈμλ‘ νμ©

    ```python
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Patient.objects.create(name='tony')
    
    doctor1.patient_set.add(patient1) # μμ¬ 1μ μ§λ£ μμ½ μ€ νμ 1 μΆκ°
    # λλ
    patient1.doctors.add(doctor1) # νμ 1μ μ§λ£ μμ½ μ€ μμ¬ 1 μΆκ°
    ```

  -  `remove()`

    - "κ΄λ ¨ κ°μ²΄ μ§ν©μμ μ§μ λ λͺ¨λΈ κ°μ²΄λ₯Ό μ κ±°"
    - λ΄λΆμ μΌλ‘ QuerySet.delete()λ₯Ό μ¬μ©νμ¬ κ΄κ³κ° μ­μ λ¨
    - λͺ¨λΈ μΈμ€ν΄μ€, νλ κ°(PK)μ μΈμλ‘ νμ©

    ```python
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Patient.objects.create(name='tony')
    
    doctor1.patient_set.remove(patient1) # μμ¬ 1μ μ§λ£ μμ½ μ€ νμ 1 μ κ±°
    patient1.doctors.remove(doctor1) # νμ 1μ μ§λ£ μμ½ μ€ μμ¬ 1 μ κ±°
    ```

  - `create()`, `clear()`, `set()` λ±

### through μμ

- λͺ¨λΈ κ΄κ³ μ€μ 

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² μμ¬ {self.name}'


class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation') # through!!
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}λ² νμ {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField() # μΆκ°νκ³  μΆμ νλ
    reserved_at = models.DateTimeField(auto_now_add=True) # μΆκ°νκ³  μΆμ νλ

    def __str__(self):
        return f'{self.doctor.pk}λ² μμ¬μ {self.patient.pk}λ² νμ'
```

- μ€κ° νμ΄λΈ νμΈ

![image-20220418222343555](Django%20-%20Model%20Relationship2.assets/image-20220418222343555.png)

- μμ¬ 1λͺκ³Ό νμ 2λͺ μμ±

```bash
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')
patient2 = Patient.objects.create(name='harry')
```

1. ```bash
   # μ§λ£ μμ½ μμ± 1
   reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
   reservation1.save()
   
   doctor1.patient_set.all() # μμ¬ 1μ μ§λ£ μμ½
   <QuerySet [<Patient: 1λ² νμ tony>]>
   
   patient1.doctors.all() # νμ 1μ μ§λ£ μμ½
   <QuerySet [<Doctor: 1λ² νμ justin>]>
   ```

2. μ€κ° νμ΄λΈ νμΈ

   ![image-20220419000608016](Django%20-%20Model%20Relationship2.assets/image-20220419000608016.png)

3. ```bash
   # μ§λ£ μμ½ μμ± 2
   patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
   
   doctor1.patient_set.all() # μμ¬ 1μ μ§λ£ μμ½
   <QuerySet [<Patient: 1λ² νμ tony>, <Patient: 2λ² νμ harry> ]>
   
   patient2.doctors.all() # νμ 1μ μ§λ£ μμ½
   <QuerySet [<Doctor: 1λ² νμ justin>]>
   ```

4. μ€κ° νμ΄λΈ νμΈ

   ![image-20220419000847237](Django%20-%20Model%20Relationship2.assets/image-20220419000847237.png)

5. ```bash
   # μμ½ μ­μ 
   doctor1.patient_set.remove(patient1)
   patient2.doctors.remove(doctor1)
   ```



> λ°μ΄ν°λ² μ΄μ€μμμ νν
>
> - Djangoλ λ€λλ€ κ΄κ³λ₯Ό λνλ΄λ μ€κ° νμ΄λΈμ λ§λ¦
> - νμ΄λΈ μ΄λ¦μ λ€λλ€ νλμ μ΄λ¦κ³Ό μ΄λ₯Ό ν¬ν¨νλ λͺ¨λΈμ νμ΄λΈ μ΄λ¦μ μ‘°ν©νμ¬ μμ±λ¨

### μ€κ° νμ΄λΈμ νλ μμ± κ·μΉ

1. `source model` λ° `target model` λͺ¨λΈμ΄ λ€λ₯Έ κ²½μ°
   - id
   - `<containing_model>_id`
   - `<other_model>_id`
2. ManyToManyFieldκ° λμΌν λͺ¨λΈμ κ°λ¦¬ν€λ κ²½μ°
   - id
   - `from_<model>_id`
   - `to_<model>_id`