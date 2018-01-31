#include <stdlib.h>
#include <unistd.h>

typedef struct meta meta_t;
struct meta{
  meta_t *next;//next block
  meta_t *prev;//previous block
  meta_t *next_f;//next free block(if the block is free)
  meta_t *prev_f;//previous block
  size_t ava;//free or not
  size_t b_size;//the block size
} ;

meta_t *start = NULL;
meta_t *start_f = NULL;
meta_t *tail = NULL;

void *ff_malloc(size_t size);
void new_brk(meta_t *curr);
void ff_free(void *ptr);
void bf_free(void *ptr);
void *bf_malloc(size_t size);
unsigned long get_data_segment_size();
unsigned long get_data_segment_free_space_size();
