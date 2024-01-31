import random
import multiprocessing
from multiprocessing import Queue
from time import sleep

'''
This program will use random points to determine the rough area of a circle given a user defined
radius. It also uses multiprocessing to speed up the computations and averages them out to effectively 
run X times faster than a normal python program. Next versions will include a function to take out
a chunk of the circle based on a Y-value height and find the area there as well, with the possibilities
for doing so with functions of cut off space.
'''

# The amount of random points for each process
random_points = 10000000
# user defined radius. Somehow works with negative radius'
radius = 1
'''
number of simultaneous multi-computations. The number after the - sign is
the amount of cores you want to not run the computations. 4 will reserve 4 Core/Threads
for your computer to do other things for example
'''
number = int(multiprocessing.cpu_count()) - 4

# initializing variables and doing pre-math
total = 0
process = []
radius = radius * 2
radsqr = radius * radius

print(f'Total Computer Core Threads: {multiprocessing.cpu_count()}')
print(f'Total number of Core Threads running the program: {number}\n')

def equation(x, y, radsqr):
    z = (x * x) + (y * y) < (radsqr)
    return z


def job(q):
    count = 0
    for i in range(random_points):
        x = random.uniform(-radius, radius)
        y = random.uniform(-radius, radius)
        if equation(x, y, radsqr) is True:
            count += 1
#        sleep(0.0000001)    if needed to slow down thermal generation
    area = (count / random_points) * (radsqr)
    q.put(area)


q = Queue()

# This will start the processes
for i in range(number):
    p = multiprocessing.Process(target=job, args=(q,))
    print(f"Starting process {i+1} to run 'job'")
    p.start()
    process.append(p)

# joining the processes back together to prevent out of order execution
for p in process:
    p.join()

# tallying up the total from the process queue
for i in range(number):
    print(f'Process {i+1} finished')
    total += q.get()

total /= number
print(f'\nThis is the area based on randomness averaged over {number} iteration(s) for the radius {radius/2}: {total}')
