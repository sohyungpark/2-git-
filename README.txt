if (ExitOutLoop == true)
{
    ExitOutLoop = false;
    break;
}
    }
}

void Output()
{
    bool ExitOutLoop = false;

    for (i = 17; i >= 0; i--)
    {
        for (j = 17; j >= 0; j--)
        {
            if (snake[i][j] == 1)
            {
                gotoxy(body[i][j].x, body[i][j].y);
                printf("■");
            }

            else
            {
                ExitOutLoop = true;
                break;
            }
        }

        if (ExitOutLoop == true)
        {
            ExitOutLoop = false;
            break;
        }
    }
}

void Move()
{
    int Time = 150;
    while (1)
    {
        Sleep(Time);
        Delete();
        MoveCoor();
        Output();
        Eat();
        GameOver();
        if (_kbhit())
            break;
    }


}

void FoodOutput()
{
    bool ExitOutLoop = false;

    srand(time(NULL));

    food.x = (rand() % 18 + 1) * 2;
    food.y = rand() % 18 + 1;

    for (i = 17; i >= 0; i--)
    {
        for (j = 17; j >= 0; j--)
        {
            if (body[i][j].x == food.x && body[i][j].y == food.y)
            {
                food.x = (rand() % 18 + 1) * 2;
                food.y = rand() % 18 + 1;
            }
        }
    }

    gotoxy(food.x, food.y);
    printf("★");
}

void Eat()
{
    if (body[17][17].x == food.x && body[17][17].y == food.y)
    {
        Score();
        BodyPlus();
        FoodOutput();
    }
}

void BodyPlus()
{
    bool ExitOutLoop = false;

    for (i = 17; i >= 0; i--)
    {
        for (j = 17; j >= 0; j--)
        {
            if (snake[i][j] == 0)
            {
                snake[i][j] = 1;
                ExitOutLoop = true;
                break;
            }
        }

        if (ExitOutLoop == true)
        {
            ExitOutLoop = false;
            break;
        }
    }
}

void Score()
{
    static int score = 0;
    score += 100;
    gotoxy(70, 20);
    printf("점수 : %d", score);
    if (score == 30000)
        Clear();
}

void GameOver()
{
    if (body[17][17].x >= 38 || body[17][17].x <= 0)
    {
        system("cls");
        printf("게임 오버");
        exit(0);
    }

    if (body[17][17].y >= 19 || body[17][17].y <= 0)
    {
        system("cls");
        printf("게임 오버");
        exit(0);
    }

    for (i = 17; i >= 0; i--)
    {
        for (j = 16; j >= 0; j--)
        {
            if (body[17][17].x == body[i][j].x &&
                body[17][17].y == body[i][j].y)
            {
                system("cls");
                printf("게임 오버");
                exit(0);
            }
        }
    }
}

void GameExplain()
{
    gotoxy(70, 10);
    puts("처음엔 왼쪽키 안됨");

    gotoxy(70, 11);
    puts("방향키 : 이동");

    gotoxy(70, 12);
    puts("머리가 몸이나 벽에 닿을시 게임오버");

    gotoxy(70, 13);
    puts("별을 먹으면 점수 +100");

    gotoxy(70, 14);
    puts("q누르면 게임종료");
}

void Clear()
{
    system("cls");
    printf("클리어");
    exit(0);
}

int main()
{
    int key;

    Map();
    CursorView(0);
    Init();
    Output();
    FoodOutput();

    while (1)
    {
        GameExplain();

        if (_kbhit())
        {
            key = _getch();

            if (key == 'q')
                break;

            switch (key)
            {
            case UP:
                if (l.Down == true)
                    l.Up = false;
                else
                {
                    l.Up = true;
                    l.Down = false;
                    l.Right = false;
                    l.Left = false;
                }
                Move();
                break;
            case DOWN:
                if (l.Up == true)
                    l.Down = false;
                else
                {
                    l.Down = true;
                    l.Up = false;
                    l.Right = false;
                    l.Left = false;
                }
                Move();
                break;
            case LEFT:
                if (body[17][17].x == 14 && body[17][17].y == 14)
                    l.Left = false;
                else if (l.Right == true)
                    l.Left = false;
                else
                {
                    l.Left = true;
                    l.Down = false;
                    l.Right = false;
                    l.Up = false;
                }
                Move();
                break;
            case RIGHT:
                if (l.Left == true)
                    l.Right = false;
                else
                {
                    l.Right = true;
                    l.Down = false;
                    l.Up = false;
                    l.Left = false;
                }
                Move();
                break;
            default:
                break;
            }
        }
    }
    return 0;
}


void gotoxy(int x, int y)
{
    COORD Pos = { x, y };
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), Pos);
}
void CursorView(char show)//커서숨기기
{
    HANDLE hConsole;
    CONSOLE_CURSOR_INFO ConsoleCursor;

    hConsole = GetStdHandle(STD_OUTPUT_HANDLE);

    ConsoleCursor.bVisible = show;
    ConsoleCursor.dwSize = 1;

    SetConsoleCursorInfo(hConsole, &ConsoleCursor);
}
