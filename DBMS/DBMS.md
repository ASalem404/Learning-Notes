- Reading 64 bit in RAM require 100 ns but in SSD require 150,000 ns but in HDD require 10,000,000 ns

- Storage manager

  - Responsible for maintaining database files
  - organize the files as collections of pages
  - track read and write to these pages and available storage space.

- Page

  - Fixed size block of data
  - 512B - 16KB
  - SQL Server, Postgres => 8KB, MySQL => 16KB
  - It contains tuples, meta-data, indexes, log records and more.
  - Some systems require the page to be self-contained (all information about the page is inside the page.)
  - Each page is given a unique identifier, the storage manager use this ID to find the page.
  - Different databases manage pages in files on disks in different ways (Heap file, tree file, hashing file)

  - Heap File

    - Unordered collection of pages where tuples stored in random pages
    - Need meta-data to know which page have free space and more.
    - Two ways to implement => page directory. linked list

      - Page directory (good one)

        - The DBMS maintains special pages that track the location of each page and a meta-data about each one (ex. number of free slots in each one)
        - The DBMS make sure that the directory pages are in sync with data pages.

      - Linked list
        - At the beginning of the file stores two things:
          - Head of the page list
          - Head of the data page list
        - each page keep track the number of free slots in itself

  - Page Header

    - Every page contains a header of meta-data about the page's contents:
      - Page size, checksum, DBMS version and compression information.
    - Some systems require pages to be self-contained (ex. Oracle)

  - Page Layout

    - We need to decide how to organise the data inside of the page. Two approaches :

      - Tuple
      - Log structured

    - Tuple

      - Strawman Idea

        - Keep track of the number of tuples in a page and then just append a new tuple to the end, it depends on that attributes or elements have a fixed size.
        - But what happen if we delete a tuple from the middle for example ?!
        - what happen if we have a variable length attributes ?

      - Slotted pages

        - The most common layout scheme
        - Header or slot array

          - each element in the array points to the start position of each tuple.
          - keeps track the number of used slots.

        - What happen if we delete a tuple from the middle for example ?

          - it depends on the implementation
            you may continue as you are and let the gap or shift the elements to remove the gap.

        - Each tuple have a unique ID, most common (page_id, slot_number)
          but we cannot depends on the ID because if we delete a slot before the ID will not change.
        - ID have different size in different DBMS
          - PostgreSQL ==> CTID(6-Bytes)
          - SQLite ==> ROWID (8-Bytes)

      - Log Structured
        - SOON

    - Tuple Layout
      - A sequence of bytes, DBMS interpret those bytes into value.
      - Each tuple is prefixed with a header contains the tuple meta-date
