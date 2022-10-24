# 🌱Django - Model Relationship 2

## 병원 진료 기록 시스템 (M:N 관계)

- 1:N 모델 관계 설정

```python
# hospitals/models.py

class Doctor(model.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'
    
class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 의사 2명과 환자 2명 생성

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

## 1:N의 한계

- 1번 환자(tony)가 1번 의사의 진료를 마치고, 2번 의사에게도 방문하려고 한다면, 새로운 예약을 생성해야 한다.
- 기존의 예약을 유지한 상태로 새로운 예약을 생성하게 되고, 새로 생성한 3번 환자(tony)는 1번 환자(tony)와 다르다!
- 어떤 환자가 한 번에 두 의사에게 진료를 받는 경우도 있고, 이를 적용하려고 하나의 외래 키에 2개의 의사 데이터를 넣을 생각을 할 수 있지만 에러가 발생 

> 1:N으로는 새로운 예약을 생성하는 것이 불가능하기 때문에 새로운 객체를 생성해야 한다. 또한 여러 의사에게 진료 받은 기록을 환자 한 명에 저장할 수 없다!

### 중개 모델

> 중개 모델 혹은 중개 테이블을 생성

```python
# hospitals/models.py

from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

    
class Patient(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'

```

- 중개 모델과의 모델 관계 확인
- 한 명의 의사가 여러 개의 진료 예약을 가질 수 있음, 마찬가지로 한명의 환자도 여러 진료 예약을 가질 수 있음

![image-20220418195602420](Django%20-%20Model%20Relationship2.assets/image-20220418195602420.png)

- 의사 1명과 환자 1명 생성 및 예약 생성

```bash
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')

Reservation.objects.create(doctor=doctor1, patient=patient1)
```

- 의사1의 예약 환자 조회

```bash
doctor1.reservation_set.all()
<QuerySet [<Reservation: 1번 의사의 1번 환자>]>
```

- 환자의 담당 의사 조회

```bash
patient1.reservation_set.all()
<QuerySet [<Reservation: 1번 의사의 1번 환자>]>
```

- 환자 1명 추가 생성 및 1번 의사에게 예약 생성

```bash
patient2 = Patient.objects.create(name='harry')
Reservation.objects.create(doctor=doctor1, patient=patient2)

doctor1.reservation_set.all()
<QuerySet [<Reservation: 1번 의사의 1번 환자>, <Reservation: 1번 의사의 2번 환자>]>
```



### ManyToManyField

> M:N(many-to-many) 관계 설정 시 사용하는 모델 필드로 하나의 필수 위치 인자(M:N 관계로 설정할 모델 클래스)가 필요!

- 기존의 중개 모델을 삭제하고 `ManyToManyField`을 생성한다. 필드 작성 위치는 Doctor 또는 Patient 모두 작성 가능하다!

```python
# hospitals/models.py

from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

class Patient(models.Model):
    # ManyToManyField 작성
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- ManyToManyField가 중개 테이블을 생성해준다!

![image-20220418202808581](Django%20-%20Model%20Relationship2.assets/image-20220418202808581.png)

- 의사 1명과 환자 2명 생성

```BASH
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')
patient2 = Patient.objects.create(name='harry')
```

1. ```bash
   # 환자 1이 doctor1에게 예약 (참조)
   patient1.doctors.add(doctor1)
   
   patient1.doctors.all() # 환자 1이 예약한 의사 목록
   <QuerySet [<Doctor: 1번 의사 justin>]>
   
   doctor1.patient_set.all() # 의사 1이 예약한 환자 목록
   <QuerySet [<Patient: 1번 환자 tony>]>
   ```

2. ```bash
   # 의사 1이 환자 2를 예약 (역참조)
   doctor1.patient_set.add(patient2)
   
   doctor1.patient_set.all() # 의사 1이 예약한 환자 목록
   <QuerySet [<Patient: 1번 환자 tony>, <Patient: 2번 환자 harry>]>
   
   patient2.doctors.all() # 환자 2가 예약한 의사 목록
   <QuerySet [<Doctor: 1번 의사 justin>]>
   
   patient1.doctors.all() # 환자 1이 예약한 의사 목록
   <QuerySet [<Doctor: 1번 의사 justin>]>
   ```

3. 중개 테이블 테이터 (`hospitals_patient_doctors`) 확인

   ![image-20220418203711030](Django%20-%20Model%20Relationship2.assets/image-20220418203711030.png)

4. ```bash
   # 환자 2가 의사 1의 진료 예약 취소 (참조)
   patient2.doctors.remove(doctor1)
   
   patient2.doctors.all() # 환자 2가 예약한 의사 목록
   <QuerySet []>
   
   doctor1.patient_set.all() # 의사 1이 예약한 환자 목록
   <QuerySet [<Patient: 1번 환자 tony>>
   ```

5. ```bash
   # 의사 1이 환자 1의 진료 예약 취소 (역참조)
   doctor1.patient_set.remove(patient1)
   
   doctor1.patient_set.all() # 의사 1이 예약한 화자 목록
   <QuerySet []>
   
   patient1.doctors.all() # 환자 1이 예약한 의사 목록
   <QuerySet []>
   ```

### related_name

- target model(관계 필드를 가지지 않은 모델)이 source model(관계 필드를 가진 모델)을 참조할 때 사용할 manager의 이름을 설정
- 즉, 역참조 시에 사용하는 manager의 이름을 설정
- ForeignKey의 `related_name`과 동일

```python
class Patient(models.Model):
    # ManyToManyField - related_name 작성
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()
```

- doctor1의 예약 환자 목록 확인 해보기 (역참조)
- `related_name` 설정 후 기존의 `_set manager`는 더 이상 사용할 수 없음

![image-20220418210717477](Django%20-%20Model%20Relationship2.assets/image-20220418210717477.png)



### Django에서 중개 모델(테이블)

- django는 `ManyToManyField`를 통해 중개 테이블을 자동으로 생성한다. 
- 그렇다면 중개 테이블을 직접 작성하는 경우는 없을까?
  - 중개 테이블을 수동으로 지정하려는 경우 `through` 옵션을 사용하여, 중개 테이블을 나타내는 Django 모델을 지정할 수 있다!
  - 가장 일반적인 용도는 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하려는 경우에 사용한다.

### 요약

- 실제 Doctor와 Patient 테이블이 변하는 것은 없음
- 1:N 관계는 완전한 종속의 관계이지만, M:N 관계는 의사에게 진찰받는 환자, 환자를 진찰하는 의사의 두가지 형태로 모두 표현이 가능한 것 



## ManyToManyField

> M:N 관계 설정 시 사용하는 모델 필드로 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요!
>
> 모델 필드의 RelatedManager(1:N 또는 M:N 관련 컨텍스트에서 사용되는 manager)를 사용하여 관련 개체를 추가, 제거 또는 만들 수 있음
>
> - add(), remove(), create(), clear()...

### Arguments

1. `related_name`

   - target model (관계 필드를 가지지 않은 모델)이 source model (관계 필드를 가진 모델)을 참조할 때(역참조 시) 사용할 manager의 이름을 설정
   - ForeignKey의 related_name과 동일

2. `through`

   - 중개 테이블을 직접 작성하는 경우, through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있음
   - 일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우 (extra data with a many-to-many relationship)에 주로 사용됨

3. `symmetrical`

   - ManyToManyField가 동일한 모델(on self)을 가리키는 정의에서만 사용
   - symmetrical=True(기본값)일 경우 Django는 person_set 매니저를 추가하지 않음
   - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델의 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함

   > 그니까 이게 무슨 소리냐면, 이때까지는 M:N의 관계가 서로 다른 테이블에서 이루어졌다. 그런데 한개의 테이블 안에서 서로 다른 데이터 끼리 M:N의 관계를 가질 수 있다. 예를들어 sns의 팔로우 기능을 생각해보면 유저끼리 서로 팔로우를 하면서 M:N의 관계를 가질 수 있는데, 이때 ManyToManyField 필드를 사용한다면 모델을 self라고 입력해야 하고 이때 기본 값이 `symmetrical=True` 이다.  이 옵션을 True로 준다면 어떤일이 생기냐, A - > B를 팔로우 했을 때 자동으로 B -> A를 팔로우게 하게 된다. 따라서 대칭을 원하지 않는 경우 False로 주자! 

   ```python
   #models.py
   from django.db import models
   
   class Person(models.Model):
       friends = models.ManyToManyField('self')
       # friends = models.ManyToManyField('self', symmetrical=False)
   ```

   

### Related Manager

- 1:N 또는  M:N 관련 켄텍스트에서 사용되는 매니저

- 같은 이름의 메서드여도 각 관계(1:N, M:N)에 따라 다르게 사용 및 동작

  - 1:N 관계에서는 target 모델 인스턴스만 사용 가능
  - M:N 관계에서는 관련된 두 객체에서 모두 사용 가능

- 메서드 종류

  - `add()`

    - "지정된 객체를 관련 객체 집합에 추가"
    - 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
    - 모델 인스턴스, 필드 값(PK)을 인자로 허용

    ```python
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Patient.objects.create(name='tony')
    
    doctor1.patient_set.add(patient1) # 의사 1의 진료 예약 중 환자 1 추가
    # 또는
    patient1.doctors.add(doctor1) # 환자 1의 진료 예약 중 의사 1 추가
    ```

  -  `remove()`

    - "관련 객체 집합에서 지정된 모델 객체를 제거"
    - 내부적으로 QuerySet.delete()를 사용하여 관계가 삭제됨
    - 모델 인스턴스, 필드 값(PK)을 인자로 허용

    ```python
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Patient.objects.create(name='tony')
    
    doctor1.patient_set.remove(patient1) # 의사 1의 진료 예약 중 환자 1 제거
    patient1.doctors.remove(doctor1) # 환자 1의 진료 예약 중 의사 1 제거
    ```

  - `create()`, `clear()`, `set()` 등

### through 예시

- 모델 관계 설정

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation') # through!!
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField() # 추가하고 싶은 필드
    reserved_at = models.DateTimeField(auto_now_add=True) # 추가하고 싶은 필드

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

- 중개 테이블 확인

![image-20220418222343555](Django%20-%20Model%20Relationship2.assets/image-20220418222343555.png)

- 의사 1명과 환자 2명 생성

```bash
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')
patient2 = Patient.objects.create(name='harry')
```

1. ```bash
   # 진료 예약 생성 1
   reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
   reservation1.save()
   
   doctor1.patient_set.all() # 의사 1의 진료 예약
   <QuerySet [<Patient: 1번 환자 tony>]>
   
   patient1.doctors.all() # 환자 1의 진료 예약
   <QuerySet [<Doctor: 1번 환자 justin>]>
   ```

2. 중개 테이블 확인

   ![image-20220419000608016](Django%20-%20Model%20Relationship2.assets/image-20220419000608016.png)

3. ```bash
   # 진료 예약 생성 2
   patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
   
   doctor1.patient_set.all() # 의사 1의 진료 예약
   <QuerySet [<Patient: 1번 환자 tony>, <Patient: 2번 환자 harry> ]>
   
   patient2.doctors.all() # 환자 1의 진료 예약
   <QuerySet [<Doctor: 1번 환자 justin>]>
   ```

4. 중개 테이블 확인

   ![image-20220419000847237](Django%20-%20Model%20Relationship2.assets/image-20220419000847237.png)

5. ```bash
   # 예약 삭제
   doctor1.patient_set.remove(patient1)
   patient2.doctors.remove(doctor1)
   ```



> 데이터베이스에서의 표현
>
> - Django는 다대다 관계를 나타내는 중개 테이블을 만듦
> - 테이블 이름은 다대다 필드의 이름과 이를 포함하는 모델의 테이블 이름을 조합하여 생성됨

### 중개 테이블의 필드 생성 규칙

1. `source model` 및 `target model` 모델이 다른 경우
   - id
   - `<containing_model>_id`
   - `<other_model>_id`
2. ManyToManyField가 동일한 모델을 가리키는 경우
   - id
   - `from_<model>_id`
   - `to_<model>_id`