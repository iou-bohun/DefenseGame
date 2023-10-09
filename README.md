![제목 없는 동영상 - Clipchamp로 제작](https://github.com/iou-bohun/DefenseGame/assets/56661597/e6321461-acd8-46a7-88d4-84939770ce45)# Realm Rush 디펜스 게임 
----------------------
## 타일 배치 
  * [ExcuteAlways]
    * Edit모드에서도 스크립트가 작동하도록 해준다.
    * 스크립트의 using선언 아래에 작성한다.
    * Update의 경우 Scene에 변경이 있을 경우 호출된다.

    ![제목 없는 동영상 - Clipchamp로 제작](https://github.com/iou-bohun/DefenseGame/assets/56661597/9373e5a8-66dc-40e5-88e2-2f6623e829c9)
## Prefab Variant
 * 부모 프리팹을 상속받는 자식 프리팹
 * 부모 프리팹의 속성을 변경할 경우 자식 프리팹도 같이 변경된다.
 * <img width="183" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/640c070a-064b-49c4-88c6-47d91d4c8233">
 * Variant 프리팹의 Variant 프리팹을 만드는것도 가능하다.
   #### 변경된 타일
   <img width="537" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/02613dfc-a93e-482d-a80d-1888d4cf77f8">

## 적 이동
  ![제목 없는 동영상 - Clipchamp로 제작](https://github.com/iou-bohun/DefenseGame/assets/56661597/05e32be7-0d59-4ff3-b31f-176b8a9ebfde)
 * 적이 지나다닐수 있는 경로를 ```List<WayPoint> path = new List<WayPoint>();``` 리스트로 지정해준다.
 ### 선형 보간/Lerp(Vector3 startvalue, Vector3 EndValue, float Percent)
 * startValue와 EndValue를 Percent로 보간한다.
   #### 코드
 * ``` c#
   foreach( WayPoint wayPoint in path )
   {
       Vector3 startPosition = transform.position;
       Vector3 endPosition = wayPoint.transform.position;
       float wayPercent = 0f;

       while(wayPercent < 1f)
       {
           wayPercent += Time.deltaTime;
           transform.position = Vector3.Lerp(startPosition, endPosition, wayPercent);
           yield return new WaitForEndOfFrame();
       }
   }
   ```
## 타워 공격 
 * Particle System
 * Collision모듈, Renderer모듈 Emmision모듈을 활성화 해주었다.
 * Collision모듈을 통해 파티클의 충돌을 설정해 주었고
   * ``` private void OnParticleCollision(GameObject other) ```
   * Collision모듈의 Send Collision Messages = (OnParticleCollision 함수를 통해 스크립트에서 파티클 충돌을 감지할 수 있게 해준다) 를 활성화 해준다.  
 * Renderer모듈을 통해 파티클 모양의 Mesh를 설정해 주었다.
   <img width="1277" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/9f84e235-ab91-42de-aec8-a97b240f3334">
   

