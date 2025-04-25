import random
import time

# Generate 1000 random sensor data points
sensor_data = [random.uniform(0, 100) for _ in range(1000)]
search_target = sensor_data[500]  # Pick a value we know exists

# Searching Algorithms
def linear_search(data, target):
    for i, value in enumerate(data):
        if value == target:
            return i
    return -1

def binary_search(data, target):
    data.sort()  # Ensure the list is sorted
    low, high = 0, len(data) - 1
    while low <= high:
        mid = (low + high) // 2
        if data[mid] == target:
            return mid
        elif data[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1

# Sorting Algorithms
def bubble_sort(data):
    arr = data.copy()
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

def selection_sort(data):
    arr = data.copy()
    for i in range(len(arr)):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

def insertion_sort(data):
    arr = data.copy()
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr

def merge_sort(data):
    if len(data) <= 1:
        return data
    mid = len(data) // 2
    left = merge_sort(data[:mid])
    right = merge_sort(data[mid:])
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

def quick_sort(data):
    if len(data) <= 1:
        return data
    pivot = data[0]
    less = [x for x in data[1:] if x <= pivot]
    greater = [x for x in data[1:] if x > pivot]
    return quick_sort(less) + [pivot] + quick_sort(greater)

def heap_sort(data):
    import heapq
    arr = data.copy()
    heapq.heapify(arr)
    return [heapq.heappop(arr) for _ in range(len(arr))]

# Measure execution times
def measure_time(func, *args):
    start = time.time()
    result = func(*args)
    end = time.time()
    return end - start, result

# Apply searching and sorting algorithms
times = {
    "Linear Search": measure_time(linear_search, sensor_data, search_target)[0],
    "Binary Search": measure_time(binary_search, sensor_data.copy(), search_target)[0],
    "Bubble Sort": measure_time(bubble_sort, sensor_data)[0],
    "Selection Sort": measure_time(selection_sort, sensor_data)[0],
    "Insertion Sort": measure_time(insertion_sort, sensor_data)[0],
    "Merge Sort": measure_time(merge_sort, sensor_data)[0],
    "Quick Sort": measure_time(quick_sort, sensor_data)[0],
    "Heap Sort": measure_time(heap_sort, sensor_data)[0],
}

times
