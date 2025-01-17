import numpy as np
import random

# Inisialisasi parameter
num_ants = 10  # Jumlah semut
num_iterations = 100  # Jumlah iterasi
alpha = 1  # Pengaruh feromon
beta = 2   # Pengaruh informasi heuristik (misalnya jarak atau biaya)
rho = 0.1  # Penguapan feromon
Q = 100     # Konstanta untuk pembaruan feromon

# Matriks jarak antara titik (misalnya pengiriman)
distance_matrix = np.array([[0, 2, 2, 5, 7],
                            [2, 0, 4, 6, 3],
                            [2, 4, 0, 3, 4],
                            [5, 6, 3, 0, 2],
                            [7, 3, 4, 2, 0]])

# Matriks feromon
pheromone_matrix = np.ones_like(distance_matrix)  # Inisialisasi feromon dengan nilai 1

# Fungsi untuk menghitung probabilitas pemilihan jalur
def calculate_probability(i, j, pheromone_matrix, distance_matrix, alpha, beta, visited):
    pheromone = pheromone_matrix[i][j] ** alpha
    heuristic = (1 / distance_matrix[i][j]) ** beta  # Heuristic based on inverse distance
    if j in visited:
        return 0  # Jika sudah dikunjungi, probabilitasnya 0
    return pheromone * heuristic

# Fungsi untuk memilih jalur berdasarkan probabilitas
def choose_next_node(current_node, pheromone_matrix, distance_matrix, alpha, beta, visited):
    probabilities = []
    for j in range(len(distance_matrix)):
        prob = calculate_probability(current_node, j, pheromone_matrix, distance_matrix, alpha, beta, visited)
        probabilities.append(prob)
    total_prob = sum(probabilities)
    probabilities = [p / total_prob for p in probabilities]  # Normalisasi probabilitas
    return np.random.choice(range(len(distance_matrix)), p=probabilities)

# Fungsi untuk memperbarui feromon setelah semua semut berjalan
def update_pheromone(pheromone_matrix, paths, distance_matrix, rho, Q):
    pheromone_matrix = (1 - rho) * pheromone_matrix  # Penguapan feromon
    for path in paths:
        path_length = sum([distance_matrix[path[i], path[i+1]] for i in range(len(path)-1)])
        pheromone_deposit = Q / path_length
        for i in range(len(path)-1):
            pheromone_matrix[path[i], path[i+1]] += pheromone_deposit
            pheromone_matrix[path[i+1], path[i]] += pheromone_deposit  # Karena jalur bersifat dua arah
    return pheromone_matrix

# Fungsi untuk menjalankan ACO
def aco_algorithm(distance_matrix, num_ants, num_iterations, alpha, beta, rho, Q):
    best_path = None
    best_path_length = float('inf')
    pheromone_matrix = np.ones_like(distance_matrix)

    for iteration in range(num_iterations):
        all_paths = []  # Menyimpan jalur yang ditempuh oleh semua semut
        all_lengths = []  # Menyimpan panjang jalur untuk setiap semut
        
        # Setiap semut mulai dari node pertama
        for ant in range(num_ants):
            visited = [0]  # Mulai dari node 0
            current_node = 0
            path = [current_node]
            
            # Semut bergerak dari satu node ke node lainnya
            while len(visited) < len(distance_matrix):
                next_node = choose_next_node(current_node, pheromone_matrix, distance_matrix, alpha, beta, visited)
                visited.append(next_node)
                current_node = next_node
                path.append(next_node)
                
            # Hitung panjang jalur dan simpan
            path_length = sum([distance_matrix[path[i], path[i+1]] for i in range(len(path)-1)])
            all_paths.append(path)
            all_lengths.append(path_length)
            
            # Jika semut menemukan jalur lebih pendek, perbarui solusi terbaik
            if path_length < best_path_length:
                best_path_length = path_length
                best_path = path

        # Update feromon berdasarkan jalur yang ditemukan
        pheromone_matrix = update_pheromone(pheromone_matrix, all_paths, distance_matrix, rho, Q)
        
        print(f"Iteration {iteration+1}, Best Path Length: {best_path_length}")
    
    return best_path, best_path_length

# Menjalankan algoritma ACO
best_path, best_path_length = aco_algorithm(distance_matrix, num_ants, num_iterations, alpha, beta, rho, Q)

# Menampilkan hasil terbaik
print("\nBest Path:", best_path)
print("Best Path Length:", best_path_length)
