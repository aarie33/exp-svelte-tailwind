<script>
  import { Link } from "svelte-navigator";
  import FilterPanel from "../Components/Table/FilterPanel.svelte";
  import TableHeaderColumn from "../Components/Table/TableHeaderColumn.svelte";
  import Search from "../Components/Table/Search.svelte";
  import Pagination from "../Components/Table/Pagination.svelte";

  let query = {
    search: null,
    page: 1,
    per_page: 10,
    sort: 'name',
    order: 'asc',
  }

  let filters = []

  function sort(e) {
    query.sort = e.detail.key
    query.order = e.detail.order
  }

  function rowNumber(index) {
    const newIndex = Number(index);
    return (
      query.page * query.per_page - query.per_page + 1 + newIndex
    );
  }

  function handleQueryChange() {
    // TODO : visit link page
    // this.$inertia.replace(window.location.pathname, {
    //   data: query,
    //   preserveScroll: true,
    //   preserveState: true,
    // });

    console.log('query changed')
  }

  function changePerPage(e) {
    query.per_page = e.detail;
    handleQueryChange();
  }

  $: handleQueryChange(query)

  function clear() {
    for (let i in filters) {
      if (get(query, filters[i].paramName) != null) {
        query = query.reduce(function(item) {
          return item.filters[i].paramName != filters[i].paramName;
        });
      }
    }
    query.search = null;
  }

  function get(object, key) {
    return object[key] ?? null;
  }

  // TODO : replace with datatable response data
  let resources = {
    data: [
      {
        id: 1,
        name: 'Abi',
        email: 'abi@gmail.com',
        phone: '123',
        address: 'Jombang',
        is_active: 'Active'
      }, {
        id: 2,
        name: 'Adi',
        email: 'adi@gmail.com',
        phone: '1234',
        address: 'Kencong',
        is_active: 'Active'
      }, {
        id: 4,
        name: 'Ali',
        email: 'ali@gmail.com',
        phone: '143',
        address: 'Jombang Jember',
        is_active: 'Active'
      }, {
        id: 5,
        name: 'Ari',
        email: 'ari@gmail.com',
        phone: '12323',
        address: 'Jombang Jember',
        is_active: 'Active'
      }
    ],
    links: [
      {
        label: 'Previous'
      },{
        label: '1'
      },{
        label: 'Next'
      }
    ],
    filters: [
      {
        componentName: "dropdown-filter",
        label: "status",
        name: "status",
        options:[
          { 
            value: 'Status',
            name: 'Status'
          }
        ],
        paramName: "filter-status",
        type: "dropdown",
        value: null
      }
    ]
  }
</script>

<h5 class="text-blue-700 mb-3">Sample CRUD Page</h5>
<div class="mb-6">
  <div class="mt-2 pt-0">
    <div class="flex justify-between flex-wrap sm:flex-no-wrap">
      <FilterPanel on:input={(e) => query = e.detail } filters={ filters } />
        
      <div class="flex-grow ml-1">
        <Search on:input={(e) => query.search = e.detail } />
      </div>
      <button
        class="
          bg-white
          text-sm text-gray-600
          hover:bg-blue-50
          hover:text-blue-600
          focus:text-blue-600
          rounded
          mx-1
          px-5
          h-9
        "
        type="button"
        on:click={clear}
      >
        Reset
      </button>
      <Link
        to="create">
        <button
          class="
            bg-blue-600
            hover:bg-blue-600
            inline-block
            flex
            items-center
            h-9
            px-2
            rounded
            float-right
            text-white
            cursor-pointer
          "
          type="button"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 6v6m0 0v6m0-6h6m-6 0H6"
            />
          </svg>
          Tambah
        </button>
      </Link>
    </div>
  </div>
</div>

<div class="overflow-visible">
  <table class="bg-white table-fixed shadow-md w-full rounded-lg text-base">
    <thead>
      <tr>
        <TableHeaderColumn 
          label={'No'}
          enableSorting={ false }
        />
        <TableHeaderColumn 
          label={'Nama'}
          enableSorting={true}
          sortingKey={'name'}
          querySort={ query.sort }
          queryOrder={ query.order }
          on:sort={sort}
        />
        <TableHeaderColumn
          label={'Phone'}
          enableSorting={true}
          sortingKey={'phone'}
          querySort={ query.sort }
          queryOrder={ query.order }
          on:sort={sort}
        />
        <TableHeaderColumn
          label={'Alamat'}
          enableSorting={true}
          sortingKey={'address'}
          querySort={ query.sort }
          queryOrder={ query.order }
          on:sort={sort}
        />
        <TableHeaderColumn
          label={'Status'}
          enableSorting={true}
          sortingKey={'is_active'}
          querySort={ query.sort }
          queryOrder={ query.order }
          on:sort={sort}
        />
        <TableHeaderColumn enableSorting={false} />
      </tr>
    </thead>
    <tbody>
      {#each resources.data as item, index }
      <tr
        class="
          bg-white
          lg:hover:bg-gray-100
          flex
          lg:table-row
          flex-row
          lg:flex-row
          flex-wrap
          lg:flex-no-wrap
          mb-10
          lg:mb-0
        "
      >
        <td class="px-3">
          <Link to="detail" class="cursor-pointer hover:no-underline text-gray-800">
            { rowNumber(index) }
          </Link>
        </td>
        <td class="px-3">
          <Link to="detail" class="cursor-pointer hover:no-underline text-gray-800">
            <div>{ item.name }</div>
            <i class="text-gray-600 text-xs">{ item.email }</i>
          </Link>
        </td>
        <td class="px-3">
          <Link to="detail" class="cursor-pointer hover:no-underline text-gray-800">
            { item.phone }
          </Link>
        </td>
        <td class="px-3">
          <Link to="detail" class="cursor-pointer hover:no-underline text-gray-800">
            { item.address || "-" }
          </Link>
        </td>
        <td class="px-3">
          <Link to="detail" class="cursor-pointer hover:no-underline text-gray-800">
            <div
              class="rounded-md text-white text-sm px-2 mb-2"
              class:bg-blue-600={ item.is_active == 'Active' }
              class:bg-gray-600={ item.is_active != 'Active' }
              >{ item.is_active }</div
            >
          </Link>
        </td>
        <td
          class="
            w-full
            lg:w-auto
            pl-3
            pb-1
            pr-3
            pt-2
            lg:p-3
            text-gray-800 text-center
            block
            lg:table-cell
            relative
            lg:static
          "
        >
          <div class="flex mb-1">
            <Link to="detail">
              <div
                class="
                  inline-flex
                  items-center
                  justify-center
                  bg-yellow-600
                  hover:bg-yellow-600
                  h-8
                  w-8
                  rounded-md
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  class="fill-current text-white h-4 w-4"
                >
                  <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
                  <path
                    fill-rule="evenodd"
                    d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>
            </Link>
            
            <Link to="create">
              <div
                class="
                  inline-flex
                  items-center
                  justify-center
                  bg-blue-600
                  hover:bg-blue-700
                  h-8
                  w-8
                  rounded-md
                  mx-1
                  cursor-pointer
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  class="fill-current text-white h-4 w-4"
                >
                  <path
                    d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"
                  />
                </svg>
              </div>
            </Link>
            <div
              class="
                inline-flex
                items-center
                justify-center
                bg-red-600
                hover:bg-red-600
                h-8
                w-8
                rounded-md
                cursor-pointer
              "
            >
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                class="fill-current text-white h-4 w-4"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M6 7C5.447 7 5 7 5 7V20C5 21.104 5.896 22 7 22H17C18.104 22 19 21.104 19 20V7C19 7 18.553 7 18 7H6ZM10 19H8V10H10V19ZM16 19H14V10H16V19ZM16.618 4L15 2H9L7.382 4H3V6H8H16H21V4H16.618Z"
                  fill="currentColor"
                />
              </svg>
            </div>
          </div>
        </td>
      </tr>
      {/each}

      {#if resources.data.length == 0 }
      <tr>
        <td colspan="6" class="text-center py-3">
          <span class="text-muted">Tidak ada data</span>
        </td>
      </tr>
      {/if}
    </tbody>
  </table>

  <Pagination
    links={ resources.links }
    perPagePptions={ [10, 20, 50, 100] }
    query={ query }
    on:changePerPage={ changePerPage }
  />
</div>