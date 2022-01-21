<script>
  import { Link } from "svelte-navigator";
</script>

<h5 class="text-blue-700 mb-3">Detail</h5>
<div class="my-2 bg-white rounded shadow p-4">
  <div class="grid grid-cols-2">
    <div class="col-span-1">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-32 w-32 mb-2 text-blue-600" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-6-3a2 2 0 11-4 0 2 2 0 014 0zm-2 4a5 5 0 00-4.546 2.916A5.986 5.986 0 0010 16a5.986 5.986 0 004.546-2.084A5 5 0 0010 11z" clip-rule="evenodd" />
      </svg>
      <div class="mb-2">
        <div class="text-xs text-gray-600">
          Name
        </div>
        <div class="text-sm">
          Tuan Muda Ari
        </div>
      </div>
    </div>
    <div class="col-span-1">
      <div class="mb-2">
        <div class="text-xs text-gray-600">
          Email
        </div>
        <div class="text-sm">
          tuanmuda.ari@dafidea.com
        </div>
      </div>
      <div class="mb-2">
        <div class="text-xs text-gray-600">
          Phone
        </div>
        <div class="text-sm">
          083737373737
        </div>
      </div>
      <div class="mb-2">
        <div class="text-xs text-gray-600">
          Address
        </div>
        <div class="text-sm">
          Dusun Krajan Gajah Mada, Kerajaan Majapahit
        </div>
      </div>
    </div>
  </div>
  
  <div class="flex justify-end w-full">
    <Link
      to="/">
      <button
        class="
          bg-white
          hover:bg-blue-50
          inline-block
          flex
          items-center
          h-9
          px-4
          rounded
          float-right
          cursor-pointer
        "
        type="button"
      >
        Kembali
      </button>
    </Link>
  </div>
</div>
