RASYID MUHAMMAD NURHAKIM
5311421062
TEKNIK ELEKTRO

TUGAS AI TIC TAC TOE

import pygame
import sys
import random

# Inisialisasi Pygame
pygame.init()

# Ukuran layar dan warna
width, height = 300, 300
screen = pygame.display.set_mode((width, height))
background_color = (255, 255, 255)
line_color = (0, 0, 0)
circle_color = (0, 255, 0)
cross_color = (255, 0, 0)

# Inisialisasi board
board = [[' ' for _ in range(3)] for _ in range(3)]

# Fungsi untuk menggambar garis pembatas
def draw_lines():
    line_width = 15
    pygame.draw.line(screen, line_color, (width // 3, 0), (width // 3, height), line_width)
    pygame.draw.line(screen, line_color, (2 * width // 3, 0), (2 * width // 3, height), line_width)
    pygame.draw.line(screen, line_color, (0, height // 3), (width, height // 3), line_width)
    pygame.draw.line(screen, line_color, (0, 2 * height // 3), (width, 2 * height // 3), line_width)

# Fungsi untuk menggambar 'O'
def draw_circle(row, col):
    radius = width // 6 - 10
    x = col * width // 3 + width // 6
    y = row * height // 3 + height // 6
    pygame.draw.circle(screen, circle_color, (x, y), radius, 3)

# Fungsi untuk menggambar 'X'
def draw_cross(row, col):
    line_width = 10
    x1 = col * width // 3 + line_width
    y1 = row * height // 3 + line_width
    x2 = (col + 1) * width // 3 - line_width
    y2 = (row + 1) * height // 3 - line_width
    pygame.draw.line(screen, cross_color, (x1, y1), (x2, y2), line_width)
    pygame.draw.line(screen, cross_color, (x1, y2), (x2, y1), line_width)

# Fungsi untuk mengecek apakah suatu kotak di papan sudah terisi
def is_valid_move(row, col):
    return board[row][col] == ' '

# Fungsi untuk mengecek apakah ada pemenang atau seri
def check_winner():
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] != ' ' or \
           board[0][i] == board[1][i] == board[2][i] != ' ':
            return True
    if board[0][0] == board[1][1] == board[2][2] != ' ' or \
       board[0][2] == board[1][1] == board[2][0] != ' ':
        return True
    return False

# Fungsi untuk mengecek apakah papan sudah penuh (seri)
def is_board_full():
    return all(board[i][j] != ' ' for i in range(3) for j in range(3))

# Fungsi untuk menampilkan pesan hasil permainan
def display_result(result):
    font = pygame.font.Font(None, 36)
    text = font.render(result, True, line_color)
    screen.blit(text, (width // 6, height // 2))

# Fungsi untuk menggambar seluruh elemen di layar
def draw_elements():
    for row in range(3):
        for col in range(3):
            if board[row][col] == 'O':
                draw_circle(row, col)
            elif board[row][col] == 'X':
                draw_cross(row, col)

# Fungsi untuk mereset papan permainan
def reset_board():
    for row in range(3):
        for col in range(3):
            board[row][col] = ' '

# Fungsi untuk mendapatkan langkah komputer secara acak
def get_computer_move():
    empty_cells = [(i, j) for i in range(3) for j in range(3) if board[i][j] == ' ']
    return random.choice(empty_cells)

# Fungsi utama permainan
# ...

# Fungsi utama permainan
def play_game():
    player_turn = True  # Player selalu mulai terlebih dahulu
    game_over = False
    vs_computer = True  # Set to False if you want to play against another player

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            if player_turn and not game_over:
                if vs_computer:
                    if event.type == pygame.MOUSEBUTTONDOWN:
                        x, y = event.pos
                        col = x // (width // 3)
                        row = y // (height // 3)

                        if is_valid_move(row, col):
                            board[row][col] = 'O'
                            player_turn = not player_turn

                            if check_winner():
                                display_result("Player wins!")
                                game_over = True
                            elif is_board_full():
                                display_result("It's a tie!")
                                game_over = True
                else:
                    # Player vs Player mode
                    if event.type == pygame.MOUSEBUTTONDOWN:
                        x, y = event.pos
                        col = x // (width // 3)
                        row = y // (height // 3)

                        if is_valid_move(row, col):
                            if player_turn:
                                board[row][col] = 'O'
                            else:
                                board[row][col] = 'X'
                            player_turn = not player_turn

                            if check_winner():
                                if player_turn:
                                    display_result("Player 1 wins!")
                                else:
                                    display_result("Player 2 wins!")
                                game_over = True
                            elif is_board_full():
                                display_result("It's a tie!")
                                game_over = True

        if not player_turn and not game_over and vs_computer:
            row, col = get_computer_move()
            board[row][col] = 'X'
            player_turn = not player_turn

            if check_winner():
                display_result("Computer wins!")
                game_over = True
            elif is_board_full():
                display_result("It's a tie!")
                game_over = True

        screen.fill(background_color)
        draw_lines()
        draw_elements()

        pygame.display.flip()

# ...

# Main loop
if __name__ == "__main__":
    play_game()