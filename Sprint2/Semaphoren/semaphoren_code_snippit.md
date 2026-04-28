### Semaphoren Code Snippit
Das ist ein Code Snippit für eine Implementierung von Semaphoren in Python. Übernommen von: https://medium.com/@divyeshjagatiya/beginners-guide-to-semaphore-in-python-339081d5197b

```python
from threading import Thread, Semaphore, current_thread
import time

class Library:

    def __init__(self, capacity) -> None:
        self.librarian = Semaphore(value=capacity)

    def request_to_read(self, book, read_time):
        person_name = current_thread().name
        self.librarian.acquire()
        try:
            print(f"{person_name} is started reading {book} for {read_time} seconds.")
            time.sleep(read_time)
            print(f"{person_name} finished.")
        finally:
            self.librarian.release()

LB = Library(capacity=4)

readers = [
    Thread(name='person-1', target=LB.request_to_read, args=('book-1', 5)),
    Thread(name='person-2', target=LB.request_to_read, args=('book-2', 15)),
    Thread(name='person-3', target=LB.request_to_read, args=('book-3', 50)),
    Thread(name='person-4', target=LB.request_to_read, args=('book-4', 7)),
    Thread(name='person-5', target=LB.request_to_read, args=('book-5', 15)),
    Thread(name='person-6', target=LB.request_to_read, args=('book-6', 25)),
    Thread(name='person-7', target=LB.request_to_read, args=('book-7', 10)),
    Thread(name='person-8', target=LB.request_to_read, args=('book-8', 13))
]
print(f"Library with capacity: {LB.librarian._value}")
print(f"Readers: {len(readers)}")

# Enter all reader at same time
for _r in readers:
    _r.start()

# wait to finish all threads.
for _r in readers:
    _r.join()

print("Completed.")
```
