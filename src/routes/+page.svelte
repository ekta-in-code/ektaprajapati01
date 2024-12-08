<script lang="ts">
  // Declare reactive states using $state
  let totalPages = $state(10);
  let currentPage = $state(1);
  let paginationRange: any = $state();


  // Define the function for pagination range
  const getPaginationRange = (total: number, current: number) => {
    let range: Array<{ value: number | string; id: string }> = [];
    if (total <= 6) {
      for (let i = 1; i <= total; i++) {
        range.push({ value: i, id: `p_${i}` });
      }
    } else {
      if (current <= 2) {
        range = [
          { value: 1, id: "p_1" },
          { value: 2, id: "p_2" },
          { value: 3, id: "p_3" },
          { value: "...", id: "d_1" },
          { value: total - 2, id: `p_${total - 2}` },
          { value: total - 1, id: `p_${total - 1}` },
          { value: total, id: `p_${total}` },
        ];
      } else if (current === 3) {
        range = [
          { value: 1, id: "p_1" },
          { value: "...", id: "d_1" },
          { value: 2, id: "p_2" },
          { value: 3, id: "p_3" },
          { value: 4, id: "p_4" },
          { value: "...", id: "d_2" },
          { value: total, id: `p_${total}` },
        ];
      } else if (current >= 4 && current <= total - 3) {
        range = [
          { value: 1, id: "p_1" },
          { value: "...", id: "d_1" },
          { value: current - 1, id: `p_${current - 1}` },
          { value: current, id: `p_${current}` },
          { value: current + 1, id: `p_${current + 1}` },
          { value: "...", id: "d_2" },
          { value: total, id: `p_${total}` },
        ];
      } else {
        range = [
          { value: 1, id: "p_1" },
          { value: 2, id: "p_2" },
          { value: 3, id: "p_3" },
          { value: "...", id: "d_1" },
          { value: total - 2, id: `p_${total - 2}` },
          { value: total - 1, id: `p_${total - 1}` },
          { value: total, id: `p_${total}` },
        ];
      }
    }
    return range;
  };

  // Reactive effect for pagination range
  $effect(() => {
    paginationRange = getPaginationRange(totalPages, currentPage);
  });

  // Page click handler
  const handlePageClick = (page: number | string) => {
    if (typeof page === "number" && page >= 1 && page <= totalPages) {
      currentPage = page;
    }
  };
</script>

<div class="pagination-container">
  <ul class="pagination">
    <!-- Previous Button -->
    <button
      aria-label="previous page"
      class="pagination-item prev-next"
      class:disabled={currentPage === 1}
      onclick={() => handlePageClick(currentPage - 1)}
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
        <path d="M15 18l-6-6 6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>

    <!-- Pagination Items -->
    {#each paginationRange as { value, id } (id)}
      <button
        class="pagination-item"
        class:active={value === currentPage}
        class:disabled={value === "..."}
        onclick={() => handlePageClick(value)}
      >
        {value}
      </button>
    {/each}

    <!-- Next Button -->
    <button
      aria-label="next page"
      class="pagination-item prev-next"
      class:disabled={currentPage === totalPages}
      onclick={() => handlePageClick(currentPage + 1)}
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
        <path d="M9 18l6-6-6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>
  </ul>
</div>


<style>

.pagination-container {
	display: flex;
	flex-direction: column;
	min-height: 100vh;
	flex: 1;
	width: 100%;
	align-items: center;
	justify-content: center;
}

  .pagination {
    display: flex;
    list-style: none;
    gap: 8px;
    align-items: center;
    padding: 0;
  }

  .pagination-item {
    min-width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    border-radius: 12px;
    font-weight: 600;
    transition: all 0.3s ease;
    user-select: none;
    color: #626262;
  }

  .pagination-item:hover:not(.disabled) {
    background-color: #f0f2f5;
    color: #7367f0;
  }

  .pagination-item.active {
    background-color: #7367f0;
    color: white;
    box-shadow: 0 0 10px rgba(115, 103, 240, 0.3);
  }

  .pagination-item.disabled {
    color: #c2c2c2;
    pointer-events: none;
    cursor: not-allowed;
  }

  .prev-next {
    background-color: #f0f2f5;
    border-radius: 12px;
  }

  .prev-next:hover:not(.disabled) {
    background-color: #7367f0;
    color: white;
  }
</style>
