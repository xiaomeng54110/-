# --*-- coding:utf-8 --*--
# python3.5
# Author:YouHan

pcb = []
n = int(input("请输入你的进程数："))


# 输入函数
def inPcb():
    i = 0
    while (i < n):
        print("**************************************")
        pName = input("请输入第 %d 个进程名：" % (i + 1))
        arriveTime = int(input("请输入到达时间:"))
        serviceTime = int(input("请输入服务时间:"))
        """将数据放入列表中
            startTime=开始时间 finishTime=完成时间 
            zzTime=周转时间 dqzzTime=带权周转时间
            AzzTime=平均周转时间 AdqzzTime=平均带权周转时间
        """
        #           进程名，到达时间，服务时间，开始，完成，周转，带权周转
        pcb.append([pName, arriveTime, serviceTime, 0, 0, 0, 0])
        i += 1


# 先来先服务算法
def FCFS():
    # 对列表按照到达时间进行升序排序  x:x[1]为依照到达时间进行排序
    pcb.sort(key=lambda x: x[1], reverse=False)
    # 计算开始、完成时间
    for i in range(n):
        if (i == 0):
            startTime = int(pcb[i][1])
            pcb[i][3] = startTime
            pcb[i][4] = startTime + int(pcb[i][2])

        elif (i > 0 and int(pcb[i - 1][4]) < int(pcb[i][1])):
            startTime = int(pcb[i][1])
            pcb[i][3] = startTime
            pcb[i][4] = startTime + int(pcb[i][2])

        else:
            startTime = pcb[i - 1][4]
            pcb[i][3] = startTime
            pcb[i][4] = startTime + int(pcb[i][2])

        i += 1
    # 计算周转、带权周转时间
    for i in range(n):
        pcb[i][5] = float(pcb[i][4] - int(pcb[i][1]))
        pcb[i][6] = float(pcb[i][5] / int(pcb[i][2]))
        i += 1
    # 计算平均周转时间和平均带权周转时间
    SzzTime = 0
    SdqzzTime = 0
    for i in range(n):
        SzzTime = float(SzzTime + float(pcb[i][5]))
        SdqzzTime = float(SdqzzTime + float(pcb[i][6]))
        AzzTime = float(SzzTime / n)
        AdqzzTime = float(SdqzzTime / n)
    # 输出结果，按照开始时间进行排序
    pcb.sort(key=lambda x: x[3], reverse=False)
    print("运行结果:")
    for i in range(n):
        print("时刻: %d 开始运行进程: %s    完成时间: %d 周转时间: %d 带权周转时间: %.2f" \
              % (int(pcb[i][3]), pcb[i][0], int(pcb[i][4]), int(pcb[i][5]), float(pcb[i][6])))
        i += 1
    print("本次调度的平均周转时间为： %.2f" % float(AzzTime))
    print("本次调度的平均带权周转时间为： %.2f" % float(AdqzzTime))


# 最短进程优先算法
def SJF():
    sjf_pcb = pcb
    i = 1
    k = 0
    # 对列表按照到达时间进行升序排序  x:x[1]为依照到达时间进行排序
    sjf_pcb.sort(key=lambda x: x[1], reverse=False)

    # 定义列表的第一项内容
    startTime0 = int(sjf_pcb[0][1])
    pcb[0][3] = startTime0
    pcb[0][4] = startTime0 + int(sjf_pcb[0][2])
    sjf_pcb[0][5] = int(sjf_pcb[0][4]) - int(sjf_pcb[0][1])
    sjf_pcb[0][6] = float(sjf_pcb[0][5]) / int(sjf_pcb[0][2])

    # 对后背队列按照服务时间排序
    temp_pcb = sjf_pcb[1:len(sjf_pcb)]  # 切片 临时存放后备队列  len(sjf_pcb)获取长度
    temp_pcb.sort(key=lambda x: x[2], reverse=False)
    sjf_pcb[1:len(sjf_pcb)] = temp_pcb

    # 进行计算
    """
    首先考虑当排序后的首个进程到达时间大于前一进程的时间，则需要选出后继进程中服务时间最短且到达时间最近的一个，即使用逐个替换的方法
    由于可能不能一次替换成功，所以使用while循环在每次替换之后进行查询，可能需要进行多次循环，直到选出下一个合适的进程
    如果是最后一个进程出现此类情况，则直接对其进行计算，用K值避免对最后一个进程的两次计算
    """
    while (i < n):
        h = 1
        # 比较到达时间和前一者的完成时间，判断是否需要进行重新排序
        while (int(sjf_pcb[i][1]) >= int(sjf_pcb[i - 1][4])):
            if (i == n - 1):  # 当最后一个进程的到达时间大于前一个进程的完成时间
                startTime = sjf_pcb[i][1]
                sjf_pcb[i][3] = startTime
                sjf_pcb[i][4] = startTime + int(sjf_pcb[i][2])
                sjf_pcb[i][5] = int(sjf_pcb[i][4]) - int(sjf_pcb[i][1])
                sjf_pcb[i][6] = float(sjf_pcb[i][5]) / int(sjf_pcb[i][2])
                k = 1  # 设置参数对比，避免一重循环之后再对末尾进程重新计算
                break
            else:  # 对进程顺序进行调换
                temp_sjf_pcb = sjf_pcb[i]
                sjf_pcb[i] = sjf_pcb[i + h]
                sjf_pcb[i + h] = temp_sjf_pcb
                h += 1

                """
                如果后面的所有进程的到达时间都大于第 i 个进程的完成时间
                则重新将i之后的进程按照服务时间排序，直接对其进行计算，同时i += 1,直接开始后面的计算              
                """
                if (h >= n - i - 1):
                    temp_pcb2 = sjf_pcb[i:len(sjf_pcb)]
                    temp_pcb2.sort(key=lambda x: x[1], reverse=False)  # 后续队列重新按照到达时间顺序进行排序
                    sjf_pcb[i:len(sjf_pcb)] = temp_pcb2

                    sjf_pcb[i][3] = int(sjf_pcb[i][1])
                    sjf_pcb[i][4] = int(sjf_pcb[i][1]) + int(sjf_pcb[i][2])
                    sjf_pcb[i][5] = int(sjf_pcb[i][4]) - int(sjf_pcb[i][1])
                    sjf_pcb[i][6] = float(sjf_pcb[i][5]) / int(sjf_pcb[i][2])

                    temp_pcb2 = sjf_pcb[i + 1:len(sjf_pcb)]
                    temp_pcb2.sort(key=lambda x: x[2], reverse=False)  # 重新按照服务时间排序
                    sjf_pcb[i + 1:len(sjf_pcb)] = temp_pcb2
                    h = 1
                    i += 1
                else:
                    continue
        if (k == 1):
            break
        else:
            startTime = sjf_pcb[i - 1][4]
            sjf_pcb[i][3] = startTime
            sjf_pcb[i][4] = startTime + int(sjf_pcb[i][2])
            sjf_pcb[i][5] = int(sjf_pcb[i][4]) - int(sjf_pcb[i][1])
            sjf_pcb[i][6] = float(sjf_pcb[i][5]) / int(sjf_pcb[i][2])

            i += 1
    # 计算平均周转时间和平均带权周转时间
    SzzTime = 0
    SdqzzTime = 0
    for i in range(n):
        SzzTime = float(SzzTime + float(pcb[i][5]))
        SdqzzTime = float(SdqzzTime + float(pcb[i][6]))
        AzzTime = float(SzzTime / n)
        AdqzzTime = float(SdqzzTime / n)
    # 输出结果，按照开始时间进行排序
    sjf_pcb.sort(key=lambda x: x[3], reverse=False)
    print("运行结果:")
    for i in range(n):
        print("时刻: %d 开始运行进程: %s    完成时间: %d 周转时间: %d 带权周转时间: %.2f" \
              % (int(sjf_pcb[i][3]), sjf_pcb[i][0], int(sjf_pcb[i][4]), int(sjf_pcb[i][5]), float(sjf_pcb[i][6])))
        i += 1
    print("本次调度的平均周转时间为： %.2f" % float(AzzTime))
    print("本次调度的平均带权周转时间为： %.2f" % float(AdqzzTime))


# 运行过程
if __name__ == '__main__':
    inPcb()
    print("算法一：先来先服务算法FCFS")
    print("算法二：最短进程优先算法SJF")
    m = 1
    while (m == 1):
        option = int(input("请输入你要选择的算法(输入1，2进行选择，其他键退出):"))
        if (option == 1):
            FCFS()
        elif (option == 2):
            SJF()
        else:
            print("退出计算")
            m = 0
