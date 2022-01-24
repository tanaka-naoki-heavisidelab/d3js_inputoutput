<script lang="ts">
  import { onMount } from "svelte";
  import * as d3 from "d3";
  import { escape } from "svelte/internal";

  let _div: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let div = _div as unknown | SVGElement;
  let ul: d3.Selection<HTMLUListElement, unknown, HTMLElement, any>;

  onMount(() => {
    // Check for the various File API support.
    if (window.File && window.FileReader && window.FileList && window.Blob) {
      /* alert(" Great success! All the File APIs are supported."); */
    } else {
      alert("The File APIs are not fully supported in this browser.");
    }

    _div = d3.select("div");
    let input = _div.append("input");
    input
      .attr("type", "file")
      .attr("id", "files1")
      .attr("name", "files1[]")
      .attr("multiple", true)
      .on("input", function (evt) {
        handleFileSelect(evt);
      });

    let output = _div.append("output");
    output.attr("id", "file_list");
    ul = _div.append("ul");
  });

  function handleFileSelect(event: Event) {
    let files = (event.target as HTMLInputElement).files;

    Array.from(files).forEach((f) => {
      let li = ul.append("li");
      li.append("strong").text(escape(f.name));
      let span = li.append("span");
      span.text(
        " (" +
          (f.type ? f.type : "n/a") +
          ") - " +
          f.size +
          " bytes, last modified: " +
          (f as any).lastModifiedDate.toLocaleDateString()
      );
    });
  }
</script>

<div bind:this={div} class="example" />

<style lang="scss">
  .example {
    padding: 10px;
    border: 1px solid #ccc;
    :global(#files1) {
      font-family: sans-serif;
      margin: 0;
      border: 0;
      padding: 0;
      vertical-align: baseline;
    }
  }
</style>
