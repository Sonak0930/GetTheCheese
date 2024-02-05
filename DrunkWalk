using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.UIElements;

public class DrunkWalk : MonoBehaviour
{
    public Vector2Int dimensions;
    public int maxTunnels = 3;
    public int maxLength = 3;

    public GameObject floor;
    public int floorWidth=10;
    public int floorLength=10;
    private (int x, int y) lastDirection;
    private (int x, int y) randomDirection;
    private int[,] map;


    int iter1 = 0;
    int iter2 = 0;
    // Start is called before the first frame update
    void Start()
    { 

        map = new int[dimensions.x, dimensions.y];
        createArray();
        GenerateMap();

        for(int i=0; i<map.GetLength(0); i++)
        {
            string line = "";
            for(int j=0; j<map.GetLength(1); j++)
            {
                line += map[i,j].ToString();
            }

            Debug.Log(line);
        }
        generateMapObject();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    

    void createArray()
    {
        for(int i=0; i<dimensions.y; i++)
        {
            for(int j=0; j<dimensions.x; j++)
            {
                map[i, j] = 0;
            }
        }
    }

    void GenerateMap()
    {
        int currentRow = UnityEngine.Random.Range(0, dimensions.x);
        int currentColumn = UnityEngine.Random.Range(0, dimensions.y);

        //Generate Random Direction.
        int randomDirectionIndex = UnityEngine.Random.Range(0, (int)DirectionEnum.Down+1);
        DirectionEnum randomDirectionEnum = (DirectionEnum)randomDirectionIndex;
        randomDirection = directionValues[randomDirectionEnum];

        bool conditionA = (randomDirection.x == -lastDirection.x) && randomDirection.y == -lastDirection.y; 
        bool conditionB = (randomDirection.x == lastDirection.x) && randomDirection.y == lastDirection.y;


        while (maxTunnels > 0 && (dimensions.x > 0 || dimensions.y > 0) && maxLength > 0)
        {
            while (conditionA || conditionB)
            {
                //Generate Random Direction.
                randomDirectionIndex = UnityEngine.Random.Range(0, (int)DirectionEnum.Down + 1);
                randomDirectionEnum = (DirectionEnum)randomDirectionIndex;
                randomDirection = directionValues[randomDirectionEnum];

                conditionA = (randomDirection.x == -lastDirection.x) && randomDirection.y == -lastDirection.y;
                conditionB = (randomDirection.x == lastDirection.x) && randomDirection.y == lastDirection.y;


                iter1++;
                if(iter1 > 100)
                {
                    break;
                }
            }

            //choose random length
            int randomLength = UnityEngine.Random.Range(1, maxLength+1);
            int tunnelLength = 0;


            while (tunnelLength < randomLength)
            {
                bool condition1 = (currentRow == 0) && (randomDirection.x == -1);
                bool condition2 = (currentColumn == 0) && (randomDirection.y == -1);
                bool condition3 = (currentRow == dimensions.x - 1) && (randomDirection.x == 1);
                bool condition4 = (currentColumn == dimensions.y - 1) && (randomDirection.y == 1);

                if (condition1 || condition2 || condition3 || condition4)
                {
                    break;
                }

                else
                {
                    map[currentRow, currentColumn] = 1;
                    currentRow += randomDirection.x;
                    currentColumn += randomDirection.y;
                    tunnelLength++;
                }
            }

            //loop finished to make tunnel or it was broken.
            if (tunnelLength > 0)
            {
                lastDirection = randomDirection;
                maxTunnels--;
            }


            if (iter2 > 100)
            {
                break;
            }
            iter2++;

        }
        
       
    }

    void generateMapObject()
    {
        for(int i=0; i< dimensions.x; i++)
        {
            for(int j=0; j<dimensions.y;  j++)
            {
                if (map[i,j] == 1)
                Instantiate(floor, new Vector3(i * floorWidth, 0, j * floorLength), Quaternion.identity);
            }
        }
    }

    public enum DirectionEnum
    {
        Left=0,
        Right=1,
        Up=2,
        Down=3
    }

    Dictionary<DirectionEnum, (int x, int y)> directionValues = new Dictionary<DirectionEnum, (int x, int y)>()
{
    { DirectionEnum.Left, (-1, 0) },
    { DirectionEnum.Right, (1, 0) },
    { DirectionEnum.Up, (0,1) },
    { DirectionEnum.Down,(0,-1) }

};

}
