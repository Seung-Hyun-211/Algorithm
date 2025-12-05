# DynamicProgramming

복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 프로그래밍 방법.<br>
부분 문제 반복과 최적 부분 구조를 가지고 있을 때 더욱 적은 시간내에 해결 가능

문제를 여러 개의 하위 문제로 나누어 풀고 결합하여 최종 목적에 도달하는것<br>
각 하위 문제를 해결해 저장하여 이후 같은 문제에 대하여 다시 계산하지 않음으로써 계산 횟수를 줄인다.

재귀함수에 저장수열을 사용하는 것 만으로 찾을 수 있지만,<br>
대다수의 문제는 훨씬 복잡한 프로그래밍을 요구한다.


### 와일드카드

<b>완전탐색</b>을 이용하는 방법

>두 문자열의 앞에서 부터 비교하여 같은 위치에 같은 문자 (혹은 와일드카드 '?') 만큼 이동<br>
>두 문자열의 마지막에 도착한 경우 true를 반환<br>
>와일드카드 ```'*'```을 만난 경우 반복문을 통해 가능한( pos+skip < s.size() ) 모든 문자를 스킵하여 재귀 호출

```
bool match(const string& w, const string& s)
{
    int pos = 0;
    while (pos < s.size() && pos < w.size() && (w[pos] == '?' || s[pos] == w[pos]))
    {
        pos++;
    }

    if (pos == w.size())
        return pos == s.size();

    if (w[pos] == '*')
    {
        for (int skip = 0; pos + skip < s.size(); skip++)
        {
            if (match(w.substr(pos+1),s.substr(pos+skip)))
            {
                return true;
            }
        }
    }
    return false;
}
```

<b>DP 재귀</b>를 이용하는 방법
> 크게 달라지는 부분은 없다.<br>
> 단지 cache를 통해 계산을 하면 저장하고 중복 계산을 하지 않을 뿐이다.<br>

```
string wild, str;
int cache[101][101];

bool MemoizedMatch(int wIdx, int sIdx)
{
    int& ret = cache[wIdx][sIdx];

    if (ret != -1)
        return ret;

    while (wIdx<wild.size() && sIdx < str.size() &&(wild[wIdx]=='?' ||str[sIdx] == wild[wIdx]))
    {
        wIdx++;
        sIdx++;
    }

    if (wIdx == wild.size())
        return ret = (sIdx == str.size());

    if (wild[wIdx] == '*')
    {
        for (int skip = 0; skip+sIdx < str.size(); skip++)
        {
            if (MemoizedMatch(wIdx + 1, sIdx + skip))
                return ret = true;
        }
    }
    return false;
}
```