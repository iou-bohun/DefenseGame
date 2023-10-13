# Realm Rush 디펜스 게임 
----------------------
## 타일 좌표 표시  
  * [ExcuteAlways]
    * Edit모드에서도 스크립트가 작동하도록 해준다.
    * 스크립트의 using선언 아래에 작성한다.
    * Update의 경우 Scene에 변경이 있을 경우 호출된다.
  * TMP
    * 3D Object > Text - textMeshPro 를 사용해주었다.
    * Shader 설정을 Overlay로 해주어 다른 오브젝트를 통과해 텍스트가 표시되도록 하였다. 

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
 * <img width="211" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/05d68e17-caf6-405c-bc34-ae100171b705"> 지정된 경로 
 * /// 오류 발생 ///
   ### 수정된 적 이동
    * 기존의 경로 탐색에서는 경로가 무작위로 정해졌다. 각각의 경로 타일에 Path 태그를 정해준뒤 이를 탐색했는데 이는 순차적으로 탐색이 되지 않았다.
    * 따라서 Path 태그를 각각의 타일이 아닌 경로 타일의 부모 오브젝트에 할당한다.
    * <img width="225" alt="image" src="https://github.com/iou-bohun/DefenseGame/assets/56661597/e34dc212-71ce-44bd-82e6-29a67aee170c">
    ``` c#
    void FindPath()
   {
       path.Clear();
       GameObject parent = GameObject.FindGameObjectWithTag("Path");
       foreach (Transform child in parent.transform)
       {
          WayPoint waypoint = child.GetComponent<WayPoint>();
           if(waypoint != null)
           {
               path.Add(waypoint);
           }
       }
   }
    ```
    * GameObject가 아닌 Transform 을 사용한 이유
    * 게임 오브젝트의 상하구조 Parent Child는 Transform에서 관리한다.
    * 게임 오브젝트는 단순히 Wrapper로 생각하면 편하다. 
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
![제목 없는 동영상 - Clipchamp로 제작 (1)](https://github.com/iou-bohun/DefenseGame/assets/56661597/c06d16c8-3e28-4122-93c9-bfe3a9b109e4)
 * 게임 화면에 표시된 지형의 좌표를 끄고 켤 수 있도록 하였다.
 * ``` public bool IsPlaceble { get { return isPlaceble; } } ``` 유니티 property기능을 이용하였다.
 * ```  waypoint = GetComponentInParent<WayPoint>(); ``` 좌표를 나타내는 스크립트에서를 GetComponentInParent를 사용하였다.
    * GetComponent vs GetComponentInParent
      * GetCopmponent 대상 오브젝트의 특정 컴포넌트들을 검색하여 모두 얻어온다.
      * GetComponentInParent 대상 오브젝트를 포함한 부모들 중에서 특정 컴포넌트를 가진 부모 오브젝트 하나의 컴포넌트를 얻어온다.
      * WayPoint 컴포넌트는 부모 오브젝트에 붙어있기 때문에 GetComponentInParent를 사용하게 된다.  
        
## 재화 시스템
 * 로직
   1. 적을 죽일경우 재화 증가
   2. 적이 마지막 포인트에 도달할 경우 재화 감소
   3. 타워 건설시 재화 감소
* Bank오브젝트를 만들어 재화를 관리하도록 한다.
#### FindObjectOfType vs GetComponent
 * 기본적으로 오브젝트에는 Transform 컴포넌트가 포함되어있다.
 * 이런 컴포넌트를 찾기 위해서는 GameObject를 알아야한다는 선행 조건이 필요하다.
 * FindObjectOfType을 이용해 다른 오브젝트의 컴포넌트에 접근할 수 있다. 

 ## Path Finding 알고리즘
  1. Breadth First Search (BFS) 
  2. Dijkstra
  3. A*

|알고리즘|항상 짧은길|이동 비용|다수의 시작점과 끝점|속도|
|------|---|---|---|---|
|BFS|O|X|O|중간|중간|
|Dijkstra|O|O|O|느림|
|a*|X|O|O|빠름|

 ## Breadth First Search 
  * 탬색 원리
    1. 탐색 방향 정하기
    2. 현제 노드를 트리에 Add
    3. 이웃 노드를 1에서 정한 탐색 방향 순서대로 Add
    4. 다음 트리로 이동
    5. 목적지가 아니면 3부터 다시 시작
    6. 목적지에 도달하면 첫 노드까지 되돌아가며 기록
    7. 기록한 노드들 반전
  * Dictionaries
    * 키 - 밸류 
     


-----------
본 후기는 유데미-스나이퍼팩토리 10주 완성 프로젝트캠프 학습 일지 후기로 작성 되었습니다. #프로젝트캠프 #프로젝트캠프후기 #유데미 #udemy #스나이퍼팩토리 #웅진씽크빅 #인사이드아웃 #IT개발캠프 #개발자부트캠프 #unity #유니티 #게임개발 #메타버스
