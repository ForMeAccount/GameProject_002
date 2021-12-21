# pizza게임의 틀을 제작하엿습니다.
# 제일 문제가 되었던 부채꼴 인식을 어떻게 할 것인가를 구현하였습니다.


### [pizza]


    public Transform target;

    public float angleRange = 45f;
    public float distance = 5f;
    public bool isCollision = false;

    Color _blue = new Color(0f, 0f, 1f, 0.2f);
    Color _red = new Color(1f, 0f, 0f, 0.2f);

    Vector3 direction;

    float dotValue = 0f;

    public Vector3 _arc_Pos;
    void Start()
    {
        this.gameObject.tag = "Untagged";

    }

    void Update()
    {
        dotValue = Mathf.Cos(Mathf.Deg2Rad * (angleRange / 2));
        direction = target.position - transform.position;
        if (direction.magnitude < distance)
        {
            if (Vector3.Dot(direction.normalized, _arc_Pos) > dotValue) // 충돌체크
            {
                isCollision = true;
                this.gameObject.tag = "Skull";
            }
            else
            {
                isCollision = false;
            }
                
        }
        else
            isCollision = false;
    }
    private void OnDrawGizmos()
    {
        Handles.color = isCollision ? _red : _blue;
        if (isCollision)
        {
            //실행
        }
        Draw_Solid(_arc_Pos);
    }


    public void Draw_Solid(Vector3 pos)
    {
        Handles.DrawSolidArc(transform.position, Vector3.up, pos, angleRange / 2, distance);
        Handles.DrawSolidArc(transform.position, Vector3.up, pos, -angleRange / 2, distance);

    }
