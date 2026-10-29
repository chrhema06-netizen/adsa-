# adsa-
1.AVL tree
class Node: 
def __init__(self, key): 
self.key = key 
self.left = None 
self.right = None 
self.height = 1 
class AVLTree: 
def get_height(self, root): 
return 0 if not root else root.height 
def get_balance(self, root): 
return 0 if not root else self.get_height(root.left) - self.get_height(root.right) 
def right_rotate(self, y): 
x = y.left 
T2 = x.right 
x.right = y 
y.left = T2 
y.height = 1 + max(self.get_height(y.left), self.get_height(y.right)) 
x.height = 1 + max(self.get_height(x.left), self.get_height(x.right)) 
return x 
def left_rotate(self, x): 
y = x.right 
T2 = y.left 
y.left = x 
x.right = T2 
x.height = 1 + max(self.get_height(x.left), self.get_height(x.right)) 
y.height = 1 + max(self.get_height(y.left), self.get_height(y.right)) 
return y 
def insert(self, root, key): 
if not root: 
return Node(key) 
elif key < root.key: 
root.left = self.insert(root.left, key) 
elif key > root.key: 
root.right = self.insert(root.right, key) 
else: 
return root  # duplicate not allowed 
root.height = 1 + max(self.get_height(root.left), self.get_height(root.right)) 
balance = self.get_balance(root) 
if balance > 1 and key < root.left.key: 
return self.right_rotate(root) 
if balance < -1 and key > root.right.key: 
return self.left_rotate(root) 
if balance > 1 and key > root.left.key: 
root.left = self.left_rotate(root.left) 
return self.right_rotate(root) 
if balance < -1 and key < root.right.key: 
root.right = self.right_rotate(root.right) 
return self.left_rotate(root) 
return root 
def min_value_node(self, node): 
current = node 
while current.left: 
current = current.left 
return current 
def delete(self, root, key): 
if not root: 
return root 
elif key < root.key: 
root.left = self.delete(root.left, key) 
elif key > root.key: 
root.right = self.delete(root.right, key) 
else: 
if not root.left: 
return root.right 
elif not root.right: 
return root.left 
temp = self.min_value_node(root.right) 
root.key = temp.key 
root.right = self.delete(root.right, temp.key) 
root.height = 1 + max(self.get_height(root.left), self.get_height(root.right)) 
balance = self.get_balance(root) 
if balance > 1 and self.get_balance(root.left) >= 0: 
return self.right_rotate(root) 
if balance > 1 and self.get_balance(root.left) < 0: 
root.left = self.left_rotate(root.left) 
return self.right_rotate(root) 
if balance < -1 and self.get_balance(root.right) <= 0: 
return self.left_rotate(root) 
if balance < -1 and self.get_balance(root.right) > 0: 
root.right = self.right_rotate(root.right) 
return self.left_rotate(root) 
return root 
def inorder(self, root, result): 
if root: 
self.inorder(root.left, result) 
result.append(root.key) 
self.inorder(root.right, result) 
if __name__ == "__main__": 
avl = AVLTree() 
root = None 
elements = [10, 20, 30, 40, 50, 25] 
print("🔹 Inserting elements:", elements) 
for num in elements: 
root = avl.insert(root, num) 
insert_val = 15 
print(f"🔹 Inserting element: {insert_val}") 
root = avl.insert(root, insert_val) 
delete_val = 20 
print(f"🔹 Deleting element: {delete_val}") 
root = avl.delete(root, delete_val) 
inorder_result = [] 
avl.inorder(root, inorder_result) 
with open("output.txt", "w") as f: 
f.write(" ".join(map(str, inorder_result))) 
print("\n✅ Final AVL Tree (Inorder Traversal):", inorder_result)

3a. Adjacency matrix:
from collections import deque 
graph_matrix = [ 
[0, 1, 1, 0], 
[1, 0, 0, 1], 
[1, 0, 0, 1],  
[0, 1, 1, 0] 
] 
def bft_matrix(graph, start): 
n=len(graph) 
visited = [False] * n 
queue = deque([start]) 
visited[start] = True 
while queue: 
v = queue.popleft() 
print(v, end=' ') 
for i in range(n): 
if graph[v][i]== 1 and not visited[i]: 
queue.append(i) 
visited[i] = True 
def dft_matrix(graph, start, visited=None): 
if visited is None: 
visited = [False] * len(graph) 
visited[start] = True 
print(start, end=' ') 
for i in range(len(graph)): 
if graph[start][i]== 1 and not visited[i]: 
dft_matrix(graph, i, visited) 
print("BFT (Adjacency Matrix):") 
bft_matrix(graph_matrix, 0) 
print("\nDFT (Adjacency Matrix):") 
dft_matrix(graph_matrix, 0)

3b. adjacency list:
from collections import deque 
graph_list = { 
0: [1, 2], 
1: [0, 3], 
2: [0, 3], 
3: [1, 2] 
} 
def bft_list(graph, start): 
visited = set() 
queue = deque([start]) 
visited.add(start) 
while queue: 
v = queue.popleft() 
print(v, end=' ') 
for neighbor in graph[v]: 
if neighbor not in visited: 
queue.append(neighbor) 
visited.add(neighbor) 
def dft_list(graph, start, visited=None): 
if visited is None: 
visited = set() 
visited.add(start) 
print(start, end=' ') 
for neighbor in graph[start]: 
if neighbor not in visited: 
dft_list(graph, neighbor, visited) 
print("\nBFT (Adjacency List):") 
bft_list(graph_list, 0) 
print("\nDFT (Adjacency List):") 
dft_list(graph_list, 0)

5.quick sort and merge sort:
import random 
import time 
def quick_sort(arr): 
if len(arr) <= 1: 
return arr 
pivot = arr[len(arr)//2] 
left = [x for x in arr if x < pivot] 
middle = [x for x in arr if x == pivot] 
right = [x for x in arr if x > pivot] 
return quick_sort(left) + middle + quick_sort(right) 
def merge_sort(arr): 
if len(arr) <= 1: 
return arr 
mid = len(arr)//2 
left = merge_sort(arr[:mid]) 
right = merge_sort(arr[mid:]) 
return merge(left, right) 
def merge(left, right): 
result = [] 
i = j = 0 
while i < len(left) and j < len(right): 
if left[i] < right[j]: 
result.append(left[i]) 
i += 1 
else: 
result.append(right[j]) 
j += 1 
result.extend(left[i:]) 
result.extend(right[j:]) 
return result 
def measure_time(sort_func, arr): 
start = time.time() 
sort_func(arr) 
end = time.time() 
return end - start 
sizes = [1000, 5000, 10000] 
cases = ["Average", "Best", "Worst"] 
for size in sizes: 
print(f"\nInput size: {size}") 
arr_avg = [random.randint(0, size) for _ in range(size)] 
arr_best = sorted(arr_avg) 
arr_worst = sorted(arr_avg, reverse=True) 
for case, arr in zip(cases, [arr_avg, arr_best, arr_worst]): 
time_quick = measure_time(quick_sort, arr.copy()) 
time_merge = measure_time(merge_sort, arr.copy()) 
print(f"{case} Case - Quick Sort: {time_quick:.5f}s, Merge Sort: {time_merge:.5f}s")

7.JOB sequenciing;
class Job: 
def __init__(self, job_id, deadline, profit): 
self.id = job_id 
self.deadline = deadline 
self.profit = profit 
def job_sequencing(jobs): 
# Step 1: Sort jobs by decreasing profit 
jobs.sort(key=lambda x: x.profit, reverse=True) 
n = max(job.deadline for job in jobs) 
result = [None] * n 
total_profit = 0 
for job in jobs: 
for slot in range(job.deadline - 1, -1, -1): 
if result[slot] is None: 
result[slot] = job.id 
total_profit += job.profit 
break 
scheduled_jobs = [job_id for job_id in result if job_id is not None] 
return scheduled_jobs, total_profit 
if __name__ == "__main__": 
jobs = [ 
Job('A', 7, 15), 
Job('B', 2, 20), 
Job('C', 5, 30), 
Job('D', 3, 18), 
Job('E', 4, 18), 
Job('F', 5, 10), 
Job('G', 2, 23), 
Job('H', 7, 16), 
Job('I', 3, 25) 
] 
scheduled_jobs, total_profit = job_sequencing(jobs) 
print("✅ Scheduled Jobs:", scheduled_jobs) 
print("💰 Total Profit:", total_profit)

9.knapsack 0/1
def knapsack_dp(values, weights, capacity): 
n = len(values) 
dp = [[0] * (capacity + 1) for _ in range(n + 1)] 
for i in range(1, n + 1): 
for w in range(capacity + 1): 
if weights[i - 1] <= w: 
dp[i][w] = max( 
dp[i - 1][w], 
values[i - 1] + dp[i - 1][w - weights[i - 1]] 
) 
else: 
dp[i][w] = dp[i - 1][w] 
return dp[n][capacity] 
values = [60, 100, 120] 
weights = [10, 20, 30] 
W = 50 
print("Maximum value in Knapsack =", knapsack_dp(values, weights, W))


11.knapsack backtracking
def knapsack_backtrack(values, weights, capacity, n): 
if n == 0 or capacity == 0: 
return 0 
if weights[n - 1] > capacity: 
return knapsack_backtrack(values, weights, capacity, n - 1) 
else: 
return max( 
values[n - 1] + knapsack_backtrack(values, weights, capacity - weights[n - 1], n - 
1), 
knapsack_backtrack(values, weights, capacity, n - 1) 
) 
values = [60, 100, 120] 
weights = [10, 20, 30] 
W = 50 
max_value = knapsack_backtrack(values, weights, W, len(values)) 
print("Maximum value in Knapsack =", max_value) 
