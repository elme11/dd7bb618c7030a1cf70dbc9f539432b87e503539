# README
def log(func):     def wrapper(*args, **kwargs):         print(f"Calling {func.__name__} with arguments {args} and {kwargs}")         result = func(*args, **kwargs)         print(f"{func.__name__} returned {result}")         return result     return wrapper  @log def add(a, b):     return a + b  add(5, 3)
