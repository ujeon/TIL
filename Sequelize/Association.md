## Association

<br>

### Association이란?

Association의 사전적 의미는

> 	1. 협회
>  	2. 연계, 유대, 제휴
>  	3. 연관, 연상

와 같은 의미를 가지고 있다. Sequelize 에서의 association은 각 모델 간의 관계를 의미한다. Sequelize에서 association의 종류는 다음과 같이 네가지 종류가 있다.

> 	1. BelongTo
>  	2. HasOne
>  	3. HasMany
>  	4. BelongToMany

<br>

### 기본 개념

Sequelize 공식 문서에서 제시하고 있는 association에 대한 기본 개념을 살펴보자.

#### Source 그리고 Target

서로 연관 지을 두 모델이 존재하고, 각 모델은 source와 target의 관계를 가지게 된다. 앞서 살펴본 네가지 종류의 association은 사실 메서드이기도 하며, 이 메서드를 호출하는 모델이 source, 인자로 전달되는 모델이 target이 된다. 

> Source.hasOne(Target)

#### Foreign Keys

Sequelize에서 모델 간에 association을 생성하면, 제약을 가진 외래 참조 키가 자동으로 생성된다. 기본적으로 외래키의 이름은 source의 id를 참조하여 sourceId와 같이 생성된다. 만일 source에 특정 데이터가 지워지면 그 데이터를 참조하고 있는 target 데이터의 외래 참조 키는 기본적으로 NULL로 업데이트 된다. 이 기본 설정은 association 메서드를 호출할 때 onUpdate나 onDelete 옵션을 부여함으로써 변경할 수 있다. 타당성 옵션은 RESTRICT, CASCADE, NO ACTION, SET DEFAULT, SET NULL이 있다.

앞서 말한 것 처럼 1:1 그리고 1:m association에서 데이터 삭제에 대한 기본 설정은 SET NULL이며, 업데이트는 CACADE이다. 이와 다르게 n:m의 관계에서 기본 설정은 CASCADE이다. 이것이 의미하는 것은, n:m association에서 한쪽의 행을 지우거나 업데이트하면 그 행을 참조하고 있는 결합 테이블의 모든 행이 업데이트 되거나 삭제 된다.

##### underscored option

Sequelize 에서는 모델에 underscored 옵션을 설정할 수 있도록 한다. 만일 이 옵션이 true라면, 모든 속성의 field 옵션을 언더스코어 된 버전의 이름으로 설정한다. 이는 association에 의해 생성된 외래키에도 적용된다.

다음과 같은 association이 있다고 가정하자.

```javascript
User.hasMany(Task);
```

위의 코드는 Task 모델에 userId 를 생성한다. 하지만, 필드는 'user_id'로 설정된다. 이 말은 열의 이름이 'user_id'로 설정된다는 의미이다.

#### Cyclic dependencies & Disabling constraints

테이블 간에 제약을 추가한다는 것은, sequelize.sync를 사용할 때 테이블이 반드시 데이터베이스에 특정 순서로 생성되어야 한다는 것을 의미한다. 만일 Task라는 모델이 User 모델을 참조하고 있다면, users 테이블은 반드시 tasks 테이블이 생성되기 전에 먼저 생성되어야 한다. 이는 때때로 순환 참조로 이어질 수 있는데, 이러한 경우에는 sequelize가 동기화 순서를 찾지 못하게 된다. 문서와 버전 관계를 상상해보자. 문서는 여러버전이 있을 수 있고, 편의상 문서는 현재 버전에 대한 참조가 있다. 이를 association으로 표현하면 다음과 같을 것이다.

```javascript
Document.hasMany(Version); // version에 documentId 속성을 추가한다.
Document.belongTo(Version, {
  as: 'Current',
  foreignKey: 'currentVersionId',
}); // currentVersionId 속성을 document에 추가한다.
```

하지만 위의 코드는 다음과 같은 에러를 발생시킨다.

>Cyclic dependency found. Document is dependent of itself. Dependency chain: documents -> versions => documents

이 오류를 해결하기 위해서는 constraints: false 옵션을  두 associations 중 하나에 전달하면 된다.

```javascript
Document.hasMany(Version);
Document.belongsTo(Version, {
  as: 'Current',
  foreignKey: 'currentVersionId',
  constraints: false
});
```

### Enforcing a foreign key reference without constraints

때때로 제약을 추가하거나, association 없이 또다른 테이블을 참조하기를 원할 때가 있다. 이러한 경우에는 참조 속성을 스키마 정의에 수동으로 추가하고, 이들 사이의 관계를 표시할 수 있다.

```javascript
class Trainer extends Model {}
Trainer.init({
  firstName: Sequelize.STRING,
  lastName: Sequelize.STRING
}, { sequelize, modelName: 'trainer' });

// Series는 외래 참조 키 trainerId  = Trainer.id를 갖게 된다.
// after we call Trainer.hasMany(series)
class Series extends Model {}
Series.init({
  title: Sequelize.STRING,
  subTitle: Sequelize.STRING,
  description: Sequelize.TEXT,
  // 'Trainer'와 FK 관계(hasMany)를 설정한다.
  trainerId: {
    type: Sequelize.INTEGER,
    references: {
      model: Trainer,
      key: 'id'
    }
  }
}, { sequelize, modelName: 'series' });

// Video will have seriesId = Series.id foreign reference key
// after we call Series.hasOne(Video)
class Video extends Model {}
Video.init({
  title: Sequelize.STRING,
  sequence: Sequelize.INTEGER,
  description: Sequelize.TEXT,
  // set relationship (hasOne) with `Series`
  seriesId: {
    type: Sequelize.INTEGER,
    references: {
      model: Series, // Can be both a string representing the table name or a Sequelize model
      key: 'id'
    }
  }
}, { sequelize, modelName: 'video' });

Series.hasOne(Video);
Trainer.hasMany(Series);
```
