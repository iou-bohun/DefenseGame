<img width="183" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/f1550a01-f193-4fd3-94d1-911cb066bcd5"># DefenseGame //RealmRush from Udemy Cousre

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
## 적 이동
 * 선형 보간/Lerp(Vector3 startvalue, Vector3 EndValue, float Percent)
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
 * Renderer모듈을 통해 파티클 모양의 Mesh를 설정해 주었다.
   <img width="1277" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/9f84e235-ab91-42de-aec8-a97b240f3334">

