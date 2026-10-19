```python
# challenge-solve.py: Mewtype
from z3 import *

data = [
    10111, 11290, 137705, 13834, 12017, 882822,
    2861, 6952, 148686, 9290, 9962, 153274, 
    13069, 9595, 871218, 9834, 10851, 1584472, 
    9149, 5765, 159511, 9289, 5093, 126222, 
    11887, 4921, 152566, 13114, 6109, 140607,
    13366, 9447, 156018, 9712, 11045, 1795540, 
    15852, 4864, 1389923, 10278, 5369, 1586542, 
    10519, 5995, 1281695, 4298, 5674, 963368, 
    10232, 5085, 880083, 10095, 15119
]
'''
[10021, 11328, 137651, 13904, 11947, 882908, 
2935, 7026, 148628, 9232, 9904, 153312, 
13143, 9505, 871272, 9776, 10809, 1584386, 
9191, 5855, 159565, 9235, 5055, 126292,
11829, 4963, 152492, 13152, 6023, 140645,
13420, 9405, 155944, 9642, 11135, 
1795470, 15798, 4954, 1389881, 10364,
5283, 1586484, 10573, 5937, 1281733, 
4240, 5744, 963442, 10146, 4999, 880009,
10037, 15189
]
'''

n = 54
known = [None] * 54
known[0] = ord('m')
known[1] = ord('o')
known[2] = ord('e')
known[3] = ord('c')
known[4] = ord('t')
known[5] = ord('f')
known[6] = ord('{')
known[53] = ord('}')
arr = [Int(f'a_{i}') for i in range(n)] 
for times in range(10000):
    # s = Solver()
    s = z3.Tactic("qfnra-nlsat").solver()

    '''
    s.add(arr[0] == ord('m'))
    s.add(arr[1] == ord('o'))
    s.add(arr[2] == ord('e'))
    s.add(arr[3] == ord('c'))
    s.add(arr[4] == ord('t'))
    s.add(arr[5] == ord('f'))
    s.add(arr[6] == ord('{'))
    s.add(arr[53] == ord('}'))

    s.add(arr[17] == ord('c'))
    s.add(arr[18] == ord('u'))
    s.add(arr[19] == ord('1'))
    s.add(arr[20] == ord('u'))
    s.add(arr[21] == ord('s'))
    s.add(arr[22] == ord('_'))
    s.add(arr[23] == ord('4'))

    s.add(arr[34] == 116)
    '''

    for i in range(54):
        if known[i] != None:
            s.add(arr[i] == known[i])




    for i in range(54):
        s.add(arr[i] >= 32, arr[i] <= 126)
    for i in range(times * 17, times * 17 + 6):
        idx0 = i % 54
        if idx0 == 53:
            continue
        idx1 = (i + 1) % 54
        idx2 = (i + 17) % 54
        s.add(Or(arr[idx0] * arr[idx0] + arr[idx1] * arr[idx1] + arr[idx2] * arr[idx2] * arr[idx2] == (data[idx0] ^ 90),
                arr[idx0] * arr[idx1] + arr[idx2] == (data[idx0] ^ 90),
                arr[idx0] + arr[idx1] + arr[idx2] * arr[idx2] == (data[idx0] ^ 90)
                ))

    print(s.check())
    '''
    if s.check() == sat:
        m = s.model()
        print([m[arr[i]] for i in range(53)])
        # print([m[arr[i]].as_long() for i in range(53)])
    '''
    if s.check() == sat:
        m = s.model()
        vals = [m[arr[i]].as_long() for i in range(n)]
        for i in range(n):
            if vals[i] != ord('!'):
                known[i] = vals[i]



        flag = ''.join(chr(v) for v in vals)
        print(flag)

    if None not in known:
        break

print(''.join(chr(v) for v in vals))
```
python expected output:
```sh
% python3 challenge-solve.py
sat
moectf{!!!!!!!!!!cu1us_!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!}
sat
moectf{!!!!!!!!!!cu1us_4!!!!!!!!!!t_z3_s!!!!!!!!!!!!!}
sat
moectf{!!!!!!!!!!cu1us_4!!!!!!!!!!t_z3_s0!!!!!!!!!!sy}
sat
moectf{!!!!!!!c@!cu1us_4!!!!!!!!!!t_z3_s0!!!!!!!!!!sy}
sat
moectf{!!!!!!!c@lcu1us_4!!!!!!!_but_z3_s0!!!!!!!!!!sy}
sat
moectf{!!!!!!!c@lcu1us_4!!!!!!!_but_z3_s0!!!!!!!_3asy}
sat
moectf{!!!!d4_c@lcu1us_4!!!!!!!_but_z3_s0!!!!!!!_3asy}
sat
moectf{!!!!d4_c@lcu1us_4!!!!4rd_but_z3_s0!!!!!!!_3asy}
sat
moectf{!!!!d4_c@lcu1us_4!!!!4rd_but_z3_s0!!!!_1s_3asy}
sat
moectf{!@mbd4_c@lcu1us_4!!!!4rd_but_z3_s0!!!!_1s_3asy}
sat
moectf{!@mbd4_c@lcu1us_4!3_h4rd_but_z3_s0!!!!_1s_3asy}
sat
moectf{!@mbd4_c@lcu1us_4!3_h4rd_but_z3_s0!v3r_1s_3asy}
sat
moectf{l@mbd4_c@lcu1us_4!3_h4rd_but_z3_s0!v3r_1s_3asy}
sat
moectf{l@mbd4_c@lcu1us_4r3_h4rd_but_z3_s0!v3r_1s_3asy}
sat
moectf{l@mbd4_c@lcu1us_4r3_h4rd_but_z3_s0lv3r_1s_3asy}
moectf{l@mbd4_c@lcu1us_4r3_h4rd_but_z3_s0lv3r_1s_3asy}
%
```