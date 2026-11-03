# ICT374 Reading Map

Page numbers are **printed** page numbers unless marked PDF. Offsets (verified against the extracted text): Stallings PDF page = printed + 1 (Ch 1-16); APUE PDF page = printed + 34. Stallings Ch 17 uses `17-n` labels (PDF 800 = 17-1).

Text extracts (gitignored): `_sources/pdftotext/` (`ICT374TopicNN.txt`, `stallings.txt`, `apue.txt`; pages split by form-feed).

Scope rule: the slides decide scope. Textbook sections below are *supporting* ranges. Question pages are candidates that the topic agent filters to what the slides cover.

| Topic | Title | Slides | Textbook chapters (printed pages) | Question pages |
|---|---|---|---|---|
| 01 | Computer System Overview (OS theory) | `ICT374Topic01.pdf`, 49 slides | Stallings Ch 1: 1.1, 1.3-1.7, pp. 30-54 | Stallings Ch 1 Review Questions and Problems, pp. 58-60 |
| 02 | Operating System Overview (OS theory) | `ICT374Topic02.pdf`, 58 slides | Stallings Ch 2: 2.1-2.4, 2.7-2.9, pp. 69-94, 101-112 | Stallings Ch 2, pp. 127-128 |
| 03 | Process Description and Control (OS theory) | `ICT374Topic03.pdf`, 55 slides | Stallings Ch 3: 3.1-3.6, pp. 131-170 | Stallings Ch 3, pp. 171-175 |
| 04 | Unix Processes and Programming | `ICT374Topic04.pdf`, 27 pages ("Page n of 27") | APUE Ch 7.1-7.9, Ch 8, Ch 13 (slide readings); core: 7.1-7.6, 8.2-8.3, 8.5-8.6, 8.9-8.10, 9.4-9.6, 13.1-13.4, pp. 197-206, 210-212, 227-255, 293-306, 463-478 | APUE Exercises Ch 7 (p. 226), Ch 8 (p. 283), Ch 9 (p. 312), Ch 13 (p. 480) |
| 05 | Unix File Systems and Programming | `ICT374Topic05.pdf`, 44 pages | APUE Ch 3.2-3.14, Ch 4 (4.2-4.5, 4.8-4.9, 4.12, 4.14-4.25), Ch 5.2-5.8, pp. 61-87, 93-156; Stallings 12.8 (UNIX file system / inodes), pp. 580-584 | APUE Exercises Ch 3 (pp. 90-91), Ch 4 (pp. 141-142), Ch 5 (p. 175) |
| 06 | Signal Handling in UNIX | `ICT374Topic06.pdf`, 29 pages | APUE Ch 10, pp. 313-380; slide also cites Stallings Table 2.6 (p. 124) and Table 6.2 (p. 316) - **check: edition mismatch possible, verify before citing** | APUE Exercises Ch 10, pp. 381-382 |
| 07 | Inter-Process Communications Mechanisms in Unix | `ICT374Topic07.pdf`, 45 pages | APUE Ch 15, pp. 533-579 (+14.2, pp. 481-484); Stallings 5.3-5.7 per slide, pp. 244-269 (use only for semaphores / message passing as taught) | APUE Exercises Ch 15, pp. 587-588; Stallings Ch 5, pp. 275-288 |
| 08 | Network Programming with Sockets | `ICT374Topic08.pdf`, 72 pages | APUE Ch 16 (pp. 589-628), 17.2 (pp. 629-635); Stallings Ch 17 (online) 17.2-17.3, pp. 17-5 to 17-15 | APUE Exercises Ch 16 (p. 628), Ch 17 (p. 670); Stallings Ch 17 end-of-chapter, pp. 17-18 to 17-20 |
| 09 | Memory Management | `ICT374Topic09.pdf`, 79 slides | Stallings Ch 7 (pp. 340-359) and Ch 8: 8.1-8.5 (pp. 371-419) | Stallings Ch 7, pp. 360-362; Ch 8, pp. 421-424 |
| 10 | Processor Scheduling | `ICT374Topic10.pdf`, 28 slides | Stallings Ch 9: 9.1-9.3, pp. 426-454 | Stallings Ch 9, pp. 455-459 |
| 11 | Concurrency and Parallelism | `ICT374Topic11.pdf`, 69 slides | Stallings 4.1-4.3 (pp. 177-194), 5.1-5.2 (pp. 232-246), 6.1-6.4 (pp. 290-308), skim rest of Ch 4-6; 1.8 (SMP/multicore, pp. 54-57), 2.4/2.6 (microkernel, pp. 92-94, 98-100) | Stallings Ch 4 (pp. 218-222), Ch 5 (pp. 275-288), Ch 6 (pp. 332-338) |

Other sources: `ICT374Review2025S2.pdf` (5 pp.; confirms OS theory = Topics 1, 2, 3, 9, 10, 11; Unix programming = Topics 4-8) and `ICT374SampleExamPaper.pdf` (16 pp.; used for a "Sample exam" question section where a question clearly belongs to a topic).

## Notes and uncertainties

- Topic 1 slide 2 and the Topic 6 slide objectives are the reading lists; the slide text, not this map, wins on conflicts.
- Sample exam is an older format than the 2025 review (25 MCQs vs 20). Used for question content only.
- Topic 6: Stallings table page citations on the slides may not match this edition; do not cite them as evidence.
- Topic 7 slide lists Stallings 5.3-5.7 broadly; monitors/readers-writers are in scope only if the slides cover them.
- Stallings Ch 17 is an online chapter at PDF 800+; its end-of-chapter pages carry `17-n` labels.
- Stallings Review Questions start at PDF pages 60 (Ch 1), 128, 173, 219, 276, 333, 361, 422, 457 (Ch 2-9); Problems follow on the same or next page. Exact question ranges per chapter are to be confirmed by each topic agent from the page text.
- No lab sheets or assignments are supplied, so none are used.
