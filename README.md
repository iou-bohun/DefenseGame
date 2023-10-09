# Realm Rush 디펜스 게임 
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
   
  <img width="211" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/05d68e17-caf6-405c-bc34-ae100171b705"> 지정된 경로 

 ### 선형 보간/Lerp(Vector3 startvalue, Vector3 EndValue, float Percent)
 * startValue와 EndValue를 Percent로 보간한다.\
 * 선형 보간을 이용해 적이 부드럽게 움직이도록 해주었다. 
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

## Debugging Tools
![Uploading 제목 없는 동영상 - Clipchamp로 제작 (1).gif…]()
 * 게임 화면에 표시된 지형의 좌표를 끄고 켤 수 있도록 하였다.
 * ``` public bool IsPlaceble { get { return isPlaceble; } } ``` 유니티 property기능을 이용하였다.
 * ```  waypoint = GetComponentInParent<WayPoint>(); ``` 좌표를 나타내는 스크립트에서를 GetComponentInParent를 사용하였다.
    * GetComponent vs GetComponentInParent
      * GetCopmponent 대상 오브젝트의 특정 컴포넌트들을 검색하여 모두 얻어온다.
      * GetComponentInParent 대상 오브젝트를 포함한 부모들 중에서 특정 컴포넌트를 가진 부모 오브젝트 하나의 컴포넌트를 얻어온다.
      * 이 게임에서 지형의 좌표를 자타내는 스크립트인 WayPoint는 모든 지형 오브젝트마다 할당이 되어있다. 이 경우 GetComponent를 사용할 경우 모든 오브젝트의 컴포넌트를 다 같이 배열의 형태로 가져오기 때문에 오류가 발생한다. 지금은 오브젝트 하나의 좌표를 나타내는 택스트를 변경하고자 하기 때문에 부모오브젝트의 컴포넌트만 가져와 이를 사용할 수 있도록 해주는 것이다. 
        
    
