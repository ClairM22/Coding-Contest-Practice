# Coding-Contest-Practice
# Former Coding Competition Entries

import os

def display_file_content(file_content, page_size, current_page, total_pages):
    max_rows, max_columns = os.get_terminal_size()

  display_rows = page_size[1]
  display_columns = page_size[0] - 5

  start_index = (current_page - 1) * display_rows
  end_index = min(start_index + display_rows, len(file_content))

    # Display title
  title = "Title: " + file_content[0]
  print(f"+{'-' * (display_columns + 2)}+")
  print(f"|{title.center(display_columns)}|")
  print(f"+{'-' * (display_columns + 2)}+")

    # Display main text with line numbers
  for i in range(start_index, end_index):
      line_number = f"{i + 1}".rjust(3)
      line = file_content[i]
      print(f"|{line_number} {line.ljust(display_columns)}|")

    # Fill remaining rows if needed
  remaining_rows = max(0, display_rows - (end_index - start_index))
  for _ in range(remaining_rows):
      print(f"|{' ' * 3}{' ' * display_columns}|")

    # Display menu and page number
  print(f"+{'-' * (display_columns + 2)}+")
  menu = "[N]ext Page [P]rev Page [E]xit reader [+/-] change page size"
  page_info = f"Page {current_page} of {total_pages}"
  print(f"|{menu.ljust(display_columns)}|")
  print(f"|{page_info.center(display_columns)}|")
  print(f"+{'-' * (display_columns + 2)}+")

def read_ember_file(file_name):
    if not file_name.endswith('.ember'):
        print("Invalid file type. Please provide an ember file.")
        return None
    try:
        with open(file_name, 'r') as file:
            content = file.readlines()
            return [line.strip() for line in content]
    except FileNotFoundError:
        print("File not found. Please provide a valid file name.")
        return None

def main():
    file_name = input("Enter the name of the ember file: ")
    while True:
        file_content = read_ember_file(file_name)
        if file_content:
            break
        file_name = input("Enter the name of the ember file: ")

    # Initialize page parameters
  current_page = 1
  total_pages = (len(file_content) - 1) // 10 + 1
  page_size = (40, 10)

  while True:
      os.system('clear' if os.name == 'posix' else 'cls')
      display_file_content(file_content, page_size, current_page, total_pages)
      user_input = input("Enter action (N/P/E/+/-): ").strip().lower()

  if user_input == 'n' and current_page < total_pages:
      current_page += 1
  elif user_input == 'p' and current_page > 1:
      current_page -= 1
  elif user_input == 'e':
      break
  elif user_input == '+':
        if page_size[0] < 72 and page_size[1] < 18:
            page_size = (page_size[0] + 4, page_size[1] + 1)
        elif user_input == '-':
            if page_size[0] > 40 and page_size[1] > 10:
                page_size = (page_size[0] - 4, page_size[1] - 1)

if __name__ == "__main__":
    main()
    
