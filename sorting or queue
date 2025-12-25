/*
  sorting_demo.cpp
  - Implements Bubble, Selection, Insertion and QuickSort
  - Counts comparisons & swaps, measures run time
  - Optional step-by-step output for small arrays (size <= 20)
  - Single-file, easy to compile and present

  Compile:
    g++ -std=c++17 -O2 sorting_demo.cpp -o sorting_demo

  Run:
    ./sorting_demo
*/

#include <bits/stdc++.h>
using namespace std;
using Clock = chrono::high_resolution_clock;

struct Stats {
  long long comparisons = 0;
  long long swaps = 0;
  double duration_ms = 0.0;
};

void printArray(const vector<int>& a) {
  for (size_t i = 0; i < a.size(); ++i) {
    if (i) cout << ' ';
    cout << a[i];
  }
  cout << '\n';
}

vector<int> generateRandomArray(int n, int minV = 1, int maxV = 100) {
  vector<int> a(n);
  random_device rd;
  mt19937 gen(rd());
  uniform_int_distribution<> dist(minV, maxV);
  for (int i = 0; i < n; ++i) a[i] = dist(gen);
  return a;
}

void maybePrintStep(const vector<int>& a, bool show) {
  if (show) printArray(a);
}

/* Bubble Sort */
void bubbleSort(vector<int> a, bool show, Stats &s) {
  auto start = Clock::now();
  int n = (int)a.size();
  for (int i = 0; i < n - 1; ++i) {
    bool swapped = false;
    for (int j = 0; j < n - 1 - i; ++j) {
      ++s.comparisons;
      if (a[j] > a[j + 1]) {
        ++s.swaps;
        swap(a[j], a[j + 1]);
        swapped = true;
      }
      maybePrintStep(a, show);
    }
    if (!swapped) break;
  }
  auto end = Clock::now();
  s.duration_ms = chrono::duration<double, milli>(end - start).count();
}

/* Selection Sort */
void selectionSort(vector<int> a, bool show, Stats &s) {
  auto start = Clock::now();
  int n = (int)a.size();
  for (int i = 0; i < n; ++i) {
    int minIdx = i;
    for (int j = i + 1; j < n; ++j) {
      ++s.comparisons;
      if (a[j] < a[minIdx]) minIdx = j;
    }
    if (minIdx != i) {
      ++s.swaps;
      swap(a[i], a[minIdx]);
    }
    maybePrintStep(a, show);
  }
  auto end = Clock::now();
  s.duration_ms = chrono::duration<double, milli>(end - start).count();
}

/* Insertion Sort */
void insertionSort(vector<int> a, bool show, Stats &s) {
  auto start = Clock::now();
  int n = (int)a.size();
  for (int i = 1; i < n; ++i) {
    int key = a[i];
    int j = i - 1;
    // shift elements greater than key
    while (j >= 0) {
      ++s.comparisons;
      if (a[j] > key) {
        a[j + 1] = a[j];
        ++s.swaps; // count as swap/shift
        --j;
      } else break;
    }
    a[j + 1] = key;
    maybePrintStep(a, show);
  }
  auto end = Clock::now();
  s.duration_ms = chrono::duration<double, milli>(end - start).count();
}

/* QuickSort with counters */
void quickSortRec(vector<int>& a, int l, int r, Stats &s, bool show) {
  if (l >= r) return;
  int pivot = a[r];
  int i = l - 1;
  for (int j = l; j < r; ++j) {
    ++s.comparisons;
    if (a[j] <= pivot) {
      ++s.swaps;
      ++i;
      swap(a[i], a[j]);
    }
  }
  ++s.swaps;
  swap(a[i + 1], a[r]);
  maybePrintStep(a, show);
  int p = i + 1;
  quickSortRec(a, l, p - 1, s, show);
  quickSortRec(a, p + 1, r, s, show);
}

void quickSort(vector<int> a, bool show, Stats &s) {
  auto start = Clock::now();
  quickSortRec(a, 0, (int)a.size() - 1, s, show);
  auto end = Clock::now();
  s.duration_ms = chrono::duration<double, milli>(end - start).count();
}

/* Utility to run a selected algorithm and print stats */
void runAndReport(const string& name, function<void(vector<int>, bool, Stats&)> algo,
                  const vector<int>& base, bool show) {
  cout << "Running " << name << "...\n";
  if (show) {
    cout << "Initial array: ";
    printArray(base);
    cout << '\n';
  }
  Stats s;
  algo(base, show, s);
  cout << "\nResult stats for " << name << ":\n";
  cout << "  Comparisons: " << s.comparisons << '\n';
  cout << "  Swaps/Shifts: " << s.swaps << '\n';
  cout << "  Time (ms): " << fixed << setprecision(3) << s.duration_ms << '\n';
  cout << "------------------------------\n\n";
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  cout << "Simple Sorting Algorithms Demo (C++)\n";
  cout << "====================================\n\n";

  while (true) {
    cout << "Menu:\n";
    cout << "  1) Generate array and run 1 algorithm\n";
    cout << "  2) Generate array and compare all algorithms\n";
    cout << "  3) Exit\n";
    cout << "Choose an option: ";
    int option;
    if (!(cin >> option)) return 0;

    if (option == 3) {
      cout << "Goodbye!\n";
      break;
    }

    if (option != 1 && option != 2) {
      cout << "Invalid option. Try again.\n\n";
      continue;
    }

    int n;
    cout << "Enter array size (recommended <= 20000 for timing; show-steps limited to <=20): ";
    cin >> n;
    if (n <= 0) { cout << "Size must be positive\n\n"; continue; }

    int minV = 1, maxV = 100;
    cout << "Enter min and max values (e.g. 1 100): ";
    cin >> minV >> maxV;
    if (minV > maxV) swap(minV, maxV);

    bool show = false;
    if (n <= 20) {
      char ch;
      cout << "Show step-by-step array after each significant step? (y/n): ";
      cin >> ch;
      show = (ch == 'y' || ch == 'Y');
    } else {
      cout << "Large array: step-by-step printing disabled.\n";
    }

    auto base = generateRandomArray(n, minV, maxV);
    cout << "\nArray generated (" << n << " elements).\n\n";

    if (option == 1) {
      cout << "Choose algorithm:\n";
      cout << "  a) Bubble Sort\n  b) Selection Sort\n  c) Insertion Sort\n  d) Quick Sort\n";
      cout << "Enter choice (a/b/c/d): ";
      char c;
      cin >> c;
      cout << '\n';
      if (c == 'a') runAndReport("Bubble Sort", bubbleSort, base, show);
      else if (c == 'b') runAndReport("Selection Sort", selectionSort, base, show);
      else if (c == 'c') runAndReport("Insertion Sort", insertionSort, base, show);
      else if (c == 'd') runAndReport("Quick Sort", quickSort, base, show);
      else cout << "Invalid algorithm choice.\n";
    } else if (option == 2) {
      // Compare all
      runAndReport("Bubble Sort", bubbleSort, base, false);
      runAndReport("Selection Sort", selectionSort, base, false);
      runAndReport("Insertion Sort", insertionSort, base, false);
      runAndReport("Quick Sort", quickSort, base, false);
      if (n <= 20 && show) {
        cout << "If you want to view step-by-step, re-run option 1 with show enabled.\n";
      }
    }
    cout << "Press Enter to continue...";
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    cin.get();
    cout << "\n";
  }

  return 0;
}