# 비아키스 틀 제작을 하였습니다.
# 맵을 간단하게 만든 뒤 기본 기믹들을 정리하여 ball, door 등을 제작하였습니다.


[텍스처 입히기 전 배경사진]
![배경 사진](./img/)


### [ball]

    public Transform player;
    public Material[] change_ball_Mat;


    private MeshRenderer _ball_Mat;
    private NavMeshAgent _nev;
    private int chage_Ball_Int;
    private int _ball_death_Count;


    void Start()
    {
        chage_Ball_Int = 0;
        _ball_death_Count = 0;

        _nev = GetComponent<NavMeshAgent>();
        _ball_Mat = GetComponent<MeshRenderer>();


        StartCoroutine(Change_Ball_Color());
    }

    void Update()
    {
        _nev.SetDestination(player.position);

        if(_ball_death_Count > 1) // 2회 팡~
        {
            //this.gameObject.SetActive(false);
            SceneManager.LoadScene("999_GamOver");
        }
    }

    IEnumerator Change_Ball_Color()
    {
        while (true)
        {
            _ball_Mat.material = change_ball_Mat[chage_Ball_Int];




            chage_Ball_Int++;
            yield return new WaitForSeconds(1.0f);

            if (chage_Ball_Int >= change_ball_Mat.Length)
            {
                chage_Ball_Int = 0;
                _ball_death_Count++;
            }
        }
    }


    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Player"))

        {
            SceneManager.LoadScene("999_GamOver");
        }

        if (other.CompareTag("Door"))
        {
            if (other.GetComponent<MeshRenderer>().material.color == this.GetComponent<MeshRenderer>().material.color)
            {
                Debug.Log("Clear"); // clear
            }
            else
            {
                SceneManager.LoadScene("999_GamOver");
            }

            Destroy(other.gameObject);// Animation
            Destroy(this.gameObject); 
        }

    }
    
    
    
### [door]
    
    public Material[] door_Change_Mat;


    private int _door_Int;
    private MeshRenderer _door_Mat;

    private void Awake()
    {
        _door_Int = Random.Range(0, door_Change_Mat.Length);
    }
    void Start()
    {
        _door_Mat = GetComponent<MeshRenderer>();

        _door_Mat.material = door_Change_Mat[_door_Int];
    }
