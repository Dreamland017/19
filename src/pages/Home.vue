<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="sidebar">
        <input
          onkeyup="search(this)"
          id="txt_search"
          type="text"
          class="sidebar-search"
          placeholder="Search..."
        />

        <a onclick="searchproduct('all')" class="sidebar-items">
          All product
        </a>

        <a onclick="searchproduct('Manga')" class="sidebar-items"> Manga </a>
        <a onclick="searchproduct('Movies')" class="sidebar-items"> Movies </a>
        <a onclick="searchproduct('Games')" class="sidebar-items"> Games </a>
      </div>
      </div>
      <div class="product">
        <div
        v-for="(p, index) in product" :key="p.id"
          @click="openProductDetail(index)"
          :class="'product-items ' + p.type"
        >
          <img class="product-img" :src="p.img" alt="" />
          <p style="font-size: 1.2vw">{{p.name}}</p>
          <p style="font-size: 0.9vw">
            {{this.numberWithCommas( p.price )}}.- THB
          </p>
        </div>
      </div>

      
    <div id="modalDesc" class="modal" style="display: none;">
        <div onclick="closeModal()" class="modal-bg"></div>
        <div class="modal-page">
            <h2>Detail</h2>
            <br>
            <div class="modaldesc-content">
                <img id="mdd-img" class="modaldesc-img" src="./imgs/chainsaw man.jpg" alt="">
                <div class="modaldesc-detail">
                    <p id="mdd-name" style="font-size: 1.5vw;">chainsaw man</p>
                    <p id="mdd-price" style="font-size: 1.2vw;">xxxx THB</p>
                    <br>
                    <p id="mdd-desc" style="color: #adadad;">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Molestiae voluptatem dolorum perspiciatis, rerum unde ullam obcaecati ex modi nemo exercitationem quae distinctio! Eligendi nisi laborum provident saepe. Quibusdam, quas dolor!</p>
                    <br>
                    <div class="btn-control">
                        <button @click="closeModal()" class="btn">
                            Close
                        </button>
                        <button @click="addtocart()" class="btn btn-buy">
                            Add to cart
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="modalCart" class="modal" style="display: none;">
        <div onclick="closeModal()" class="modal-bg"></div>
        <div class="modal-page">
            <h2>My cart</h2>
            <br>
            <div id="mycart" class="cartlist">
                <div class="cartlist-items">
                    <div class="cartlist-left">
                        <img src="./imgs/chainsaw man.jpg" alt="">
                        <div class="cartlist-detail">
                            <p style="font-size: 1.5vw;">chainsaw man</p>
                            <p style="font-size: 1.2vw;">xxxx THB</p>
                        </div>
                    </div>
                    <div class="cartlist-right">
                        <p class="btnc">-</p>
                        <p style="margin: 0 20px;">xxxx</p>
                        <p class="btnc">+</p>
                    </div>
                </div>
                <div class="cartlist-items">
                    <div class="cartlist-left">
                        <img src="./imgs/john wick.jpg" alt="">
                        <div class="cartlist-detail">
                            <p style="font-size: 1.5vw;">John wick</p>
                            <p style="font-size: 1.2vw;">xxxx THB</p>
                        </div>
                    </div>
                    <div class="cartlist-right">
                        <p class="btnc">-</p>
                        <p style="margin: 0 20px;">xxxx</p>
                        <p class="btnc">+</p>
                    </div>
                </div>
                <div class="cartlist-items">
                    <div class="cartlist-left">
                        <img src="./imgs/the last of us.jpg" alt="">
                        <div class="cartlist-detail">
                            <p style="font-size: 1.5vw;">The last of us</p>
                            <p style="font-size: 1.2vw;">xxxx THB</p>
                        </div>
                    </div>
                    <div class="cartlist-right">
                        <p class="btnc">-</p>
                        <p style="margin: 0 20px;">xxxx</p>
                        <p class="btnc">+</p>
                    </div>
                </div>
            </div>
            <div class="btn-control">
                <button onclick="closeModal()" class="btn">
                    Cancel
                </button>
                <button class="btn btn-buy">
                    Buy
                </button>
            </div>
        </div>
    </div>
    

  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import $ from "jquery";

export default {
  components: { Navbar },
  methods: {
    numberWithCommas(x) {
      x = x.toString();
      var pattern = /(-?\d+)(\d{3})/;
      while (pattern.test(x)) x = x.replace(pattern, "$1,$2");
      return x;
    },
    search(elem) {
      var value = $("#" + elem.id).val();
      console.log(value);

      if (html == "") {
        $("#productlist").html("<p>Not found product</p>");
      } else {
        $("#productlist").html(html);
      }
    },

    searchproduct(parem) {
      console.log(parem);
      $(".product-items").css("display", "none");
      if (parem == "all") {
        $(".product-items").css("display", "block");
      } else {
        $("." + parem).css("display", "block");
      }
    },
    openProductDetail(index) {
      this.productindex = index;
      console.log(this.productindex);
      $("#modalDesc").css("display", "flex");
      $("#mdd-img").attr("src", this.product[index].img);
      $("#mdd-name").text(this.product[index].name);
      $("#mdd-price").text(
        this.numberWithCommas(this.product[index].price) + ".- THB"
      );
      $("#mdd-desc").text(this.product[index].description);
    },
    closeModal() {
      $(".modal").css("display", "none");
    },
    addtocart() {
      var pass = true;

      for (let i = 0; i < this.cart.length; i++) {
        if (this.productindex == this.cart[i].index) {
          console.log("found same product");
          this.cart[i].count++;
          pass = false;
        }
      }

      if (pass) {
        var obj = {
          index: this.productindex,
          id: this.product[this.productindex].id,
          name: this.product[this.productindex].name,
          price: this.product[this.productindex].price,
          img: this.product[this.productindex].img,
          count: 1,
        };
        // console.log(obj)
        this.cart.push(obj);
      }

      this.$swal.fire({
        icon: "success",
        title: "Add " + this.product[this.productindex].name + " to cart !",
      });
      $("#cartcount").css("display", "flex").text(this.cart.length);
    },
    openCart() {
      $("#modalCart").css("display", "flex");
      rendercart();
    },
    rendercart() {
      if (this.cart.length > 0) {
        var html = "";
        for (let i = 0; i < this.cart.length; i++) {
          html += `<div class="cartlist-items">
            <div class="cartlist-left">
                <img src="${this.cart[i].img}" alt="">
                <div class="cartlist-detail">
                    <p style="font-size: 1.5vw;">${this.cart[i].name}</p>
                    <p style="font-size: 1.2vw;">${numberWithCommas(
                      this.cart[i].price * this.cart[i].count
                    )}.- THB</p>
                </div>
            </div>
            <div class="cartlist-right">
                <p onclick="deinitems('-', ${i})" class="btnc">-</p>
                <p id="countitems${i}" style="margin: 0 20px;">${
            this.cart[i].count
          }</p>
                <p onclick="deinitems('+', ${i})" class="btnc">+</p>
            </div>
        </div>`;
        }
        $("#mycart").html(html);
      } else {
        $("#mycart").html("<p>Not found product list</p>");
      }
    },
    deinitems(action, index) {
      if (action == "-") {
        if (this.cart[index].count > 0) {
          this.cart[index].count--;
          $("#countitems" + index).text(this.cart[index].count);

          if (this.cart[index].count <= 0) {
            Swal.fire({
              icon: "warning",
              title: "Are you sure to delete?",
              showConfirmButton: true,
              showCancelButton: true,
              confirmButtonText: "Delete",
              cancelButtonText: "cancel",
            }).then((res) => {
              if (res.isConfirmed) {
                this.cart.splice(index, 1);
                console.log(cart);
                rendercart();
                $("#cartcount").css("display", "flex").text(this.cart.length);

                if (this.cart.length <= 0) {
                  $("#cartcount").css("display", "none");
                }
              } else {
                this.cart[index].count++;
                $("#countitems" + index).text(this.cart[index].count);
              }
            });
          }
        }
      } else if (action == "+") {
        this.cart[index].count++;
        $("#countitems" + index).text(this.cart[index].count);
      }
    },
  },
  data() {
    return {
      cart: [],
      productindex: 0,
      product: [
        {
          id: 1,
          img: "./imgs/chainsaw man.jpg",
          name: "Chainsaw man",
          price: 69,
          description:
            "Chainsaw man Lorem ipsum dolor sit amet consectetur adipisicing elit. Repellat mollitia ad harum provident, ut magnam.",
          type: "Manga",
        },
        {
          id: 2,
          img: "./imgs/john wick.jpg",
          name: "John wick",
          price: 190,
          description:
            "John wick Lorem ipsum dolor sit amet consectetur adipisicing elit. Corporis quam doloribus nisi laboriosam nobis dolorem!",
          type: "Movies",
        },
        {
          id: 3,
          img: "./imgs/the last of us.jpg",
          name: "The last of us",
          price: 1590,
          description:
            "The last of us Lorem ipsum dolor sit amet consectetur adipisicing elit. Corporis quam doloribus nisi laboriosam nobis dolorem!",
          type: "Games",
        },
      ],
    };
  },
  mounted() {
  },
};
</script>

<style></style>
