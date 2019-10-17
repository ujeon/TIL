## Margin Collapse에 대해 알아보기

padding은 엘리먼트 border 안쪽에 추가 되는 공간을 의미하며, margin은 border의 바깥쪽 공간을 의미합니다. 

두 속성의 차이점 중 하나는 위와 아래의 margin은 padding과는 다르게 붕괴 (collapse)되는 특징이 있습니다.



왼쪽과 오른쪽 margin은 padding처럼 항상 함께 표시되고 추가됩니다. 다음 예시는 img-one, img-two라는 id를 가진 두개의 img 엘리먼트입니다.

```css
#img-one {  
  margin-right: 20px; 
} 

#img-two {  
  margin-left: 20px; 
}
```



위의 예제에서 두 개의 엘리먼트는 서로의 margin을 합한 것 만큼 떨어져서 표시됩니다. 즉, 두 엘리먼트의 margin의 합인 40px 만큼 떨어지게 되는 것이죠.



수평 margin과는 달리 수직 margin은 더해지지 않습니다. 대신에 두 엘리먼트의 margin 중 값이 큰 margin이 두 엘리먼트 사이의 거리를 결정하게 됩니다.

```css
#img-one {  
  margin-bottom: 30px; 
} 

#img-two { 
  margin-top: 20px; 
}
```

위의 예제에서는 img-one 엘리먼트의 margin이 더 크므로 두 엘리먼트는 30px 만큼 떨어지게 됩니다.



위에서 설명한 내용을 도식화하면 아래의 그림과 같습니다.

![https://github.com/ujeon/TIL/blob/master/src/image/CSS/vertical%20margins%20collapse.png](https://github.com/ujeon/TIL/blob/master/src/image/CSS/vertical%20margins%20collapse.png)