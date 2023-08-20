<template>
  <div>
    <h1>Secret</h1>
    <p>This should never render. You should see error.vue instead.</p>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  middleware: [
    function (to, from) {
      // Attempt 1
      // This produces an inconsistent error user experience between
      // the server and client.
      // server-side: it works as expected
      // client-side: it prevents navigation, and no error is displayed

      // throw createError({ statusCode: 403, statusMessage: 'Forbidden', fatal: true });



      // Attempt 2
      // This produces an inconsistent error user experience between
      // the server and client.
      // server-side: it works as expected
      // client-side: navigation works, no error is displayed, and the forbidden page is incorrectly rendered

      try {
        throw createError({ statusCode: 403, statusMessage: 'Forbidden', fatal: true });
      } catch (error) {
        if (process.client) {
          // try to render error for client ux. this doesn't work
          showError(error);
          // return undefined to not interrupt the navigation
        }
        // rethrow for server ux
        else throw error;
      }
    },
  ],
});
</script>

