/*
by liukang 2020-8-22
机械创新大赛-智能轮椅控制电路
*/
//I/O口定义
int zhiwen = 2;
int button = 3;
int reset = 4;
int moshi1 = 5;
int moshi2 = 6;
int yuyin1 = 7;
int yuyin2 = 8;
int yuyin3 = 9;
int yuyin4 = 10;
int chaoshengbo = 11;
int yaogan = 12;
int lanya = A0;
//变量定义
int kaiguan = 0;
int modejishu = 0;
void setup()
{
    Serial.begin(9600);
    pinMode(zhiwen, INPUT);
    pinMode(button, INPUT);
    pinMode(yuyin1, OUTPUT);
    pinMode(yuyin2, OUTPUT);
    pinMode(yuyin3, OUTPUT);
    pinMode(yuyin4, OUTPUT);
    pinMode(reset, OUTPUT);
    pinMode(moshi1, OUTPUT);
    pinMode(moshi2, OUTPUT);
    pinMode(chaoshengbo, OUTPUT);
    pinMode(yaogan, OUTPUT);
    pinMode(lanya, OUTPUT);
}

void loop()
{
    if (digitalRead(zhiwen) == 1)
    {
        delay(500);
        if (digitalRead(zhiwen) == 1)
        {
            kaiguan += 1;
            kaiguan = kaiguan % 2;
            if (kaiguan == 1)
            {
                Serial.println("开机");
                digitalWrite(yuyin1, HIGH); //播放开机声音,当前模式
            }
            else if (kaiguan == 0)
            {
                Serial.println("关机");
                digitalWrite(yuyin1, HIGH); //播放关机声音
            }
            delay(5000);
            digitalWrite(yuyin1, LOW);
        }
    }
    if (kaiguan == 1)
    {
        if (digitalRead(button) == 1)
        {
            delay(500);
            if (digitalRead(button) == 1)
            {
                modejishu += 1;
                modejishu = modejishu % 4;
                Serial.print("切换模式");
                Serial.println(modejishu);
                digitalWrite(reset, 1);  //单片机复位
                moshiqiehuan(modejishu); //动作
                delay(1000);
            }
            delay(100);
        }
    }
    delay(50);
}
void moshiqiehuan(int modejishu)
{
    if (modejishu == 0) //模式切换
    {
        digitalWrite(moshi1, 0);
        digitalWrite(moshi2, 0); //播放切换模式的语音
    }
    if (modejishu == 1)
    {
        digitalWrite(moshi1, 0);
        digitalWrite(moshi2, 1);
    }
    if (modejishu == 2)
    {
        digitalWrite(moshi1, 1);
        digitalWrite(moshi2, 0);
    }
    if (modejishu == 3)
    {
        digitalWrite(moshi1, 1);
        digitalWrite(moshi2, 1);
    }
}
