
document.addEventListener("DOMContentLoaded", () => {
  const SUPABASE_URL = "https://syuwgevbyxnpowbizvdc.supabase.co";
const SUPABASE_PUBLISHABLE_KEY = "sb_publishable_YzTt8hzawZ8_80hL9mJwgA_D16y1cCE";
// แก้ชื่อ ประวัติ และ URL รูปโปรไฟล์เจ้าของลิงก์ได้จากตรงนี้
  const OWNER_PROFILE = {
    name: "pxtiphh._", // ใส่ชื่อเจ้าของลิงก์ที่นี่
    handle: "@your_username",
    bio: "เขียนข้อความถึงเราได้ที่นี่",
    avatarUrl: "image4/zkk.jpg" // ใส่ URL ของรูปโปรไฟล์เจ้าของลิงก์ที่นี่ (ถ้าไม่มีให้เว้นว่าง),
  };

  const home = document.getElementById("home");
  const compose = document.getElementById("compose");
  const result = document.getElementById("result");

  const visitorForm = document.getElementById("visitor-form");
  const visitorNameInput = document.getElementById("visitor-name");
  const usernameError = document.getElementById("username-error");
  const homeProfileName = document.getElementById("home-profile-name");
  const homeProfileHandle = document.getElementById("home-profile-handle");
  const homeProfileBio = document.getElementById("home-profile-bio");
  const homeProfileInitial = document.getElementById("home-profile-initial");
  const homeProfileImage = document.getElementById("home-profile-image");
  const profileName = document.getElementById("profile-name");
  const profileBio = document.getElementById("profile-bio");
  const profileInitial = document.getElementById("profile-initial");
  const profileImage = document.getElementById("profile-image");

  const messageForm = document.getElementById("message-form");
  const messageInput = document.getElementById("message");
  const count = document.getElementById("count");

  const senderWrap = document.getElementById("sender-wrap");
  const senderInput = document.getElementById("sender");
  const formError = document.getElementById("form-error");

  const previewMessage = document.getElementById("preview-message");
  const previewSender = document.getElementById("preview-sender");

  const backButton = document.getElementById("back");
  const againButton = document.getElementById("again");
  const startOverButton = document.getElementById("start-over");
  const homeLink = document.getElementById("home-link");
  const footerHome = document.getElementById("footer-home");
  let visitorName = "";

  profileName.textContent = OWNER_PROFILE.name;
  profileBio.textContent = OWNER_PROFILE.bio;
  profileInitial.textContent = OWNER_PROFILE.name.trim().charAt(0).toUpperCase() || "V";
  homeProfileName.textContent = OWNER_PROFILE.name;
  homeProfileHandle.textContent = OWNER_PROFILE.handle;
  homeProfileBio.textContent = OWNER_PROFILE.bio;
  homeProfileInitial.textContent = OWNER_PROFILE.name.trim().charAt(0).toUpperCase() || "V";

  if (OWNER_PROFILE.avatarUrl) {
    profileImage.src = OWNER_PROFILE.avatarUrl;
    profileImage.classList.remove("hidden");
    homeProfileImage.src = OWNER_PROFILE.avatarUrl;
    homeProfileImage.classList.remove("hidden");
  }

  [profileImage, homeProfileImage].forEach((image) => {
    image.addEventListener("error", () => {
      console.error("Unable to load the owner profile image:", image.src);
      image.classList.add("hidden");
    });
  });

  function showPage(page) {
    home.classList.toggle("hidden", page !== "home");
    compose.classList.toggle("hidden", page !== "compose");
    result.classList.toggle("hidden", page !== "result");
    document.querySelector(".container").classList.toggle("is-composing", page === "compose");

    window.scrollTo({ top: 0, behavior: "smooth" });
  }

  function clearError() {
    formError.textContent = "";
    formError.classList.add("hidden");
  }

  function clearUsernameError() {
    usernameError.textContent = "";
    usernameError.classList.add("hidden");
  }

  function showUsernameError(message) {
    usernameError.textContent = message;
    usernameError.classList.remove("hidden");
  }

  function showError(message) {
    formError.textContent = message;
    formError.classList.remove("hidden");
  }

  function resetComposer(name = "") {
    messageForm.reset();
    messageInput.value = "";
    count.textContent = "0";
    senderInput.value = name;
    document.querySelector('input[name="mode"][value="named"]').checked = Boolean(name);
    document.querySelector('input[name="mode"][value="anonymous"]').checked = !name;
    senderWrap.classList.toggle("hidden", !name);
    clearError();
  }

  
  visitorForm.addEventListener("submit", (event) => {
    event.preventDefault();
    clearUsernameError();

    const username = visitorNameInput.value;

    if (!/^(?=.*[A-Za-z._])[A-Za-z0-9._]{5,30}$/.test(username)) {
      showUsernameError("ชื่อไม่ถูกต้อง");
      return;
    }

    visitorName = username;
    resetComposer(`@${visitorName}`);
    showPage("compose");
    messageInput.focus({ preventScroll: true });
  });

  backButton.addEventListener("click", () => {
    clearError();
    showPage("home");
    visitorNameInput.focus({ preventScroll: true });
  });

  // นับจำนวนตัวอักษร
  messageInput.addEventListener("input", () => {
    count.textContent = String(messageInput.value.length);
    clearError();
  });

  // สลับโหมดไม่เปิดเผยชื่อ / แสดงชื่อที่กรอก
  const modeInputs = document.querySelectorAll('input[name="mode"]');

  modeInputs.forEach((input) => {
    input.addEventListener("change", () => {
      const namedMode = input.checked && input.value === "named";

      if (input.checked) {
        senderWrap.classList.toggle("hidden", !namedMode);
      }

      clearError();

      if (namedMode) {
        senderInput.focus({ preventScroll: true });
      }
    });
  });

  senderInput.addEventListener("input", clearError);

    // แสดงตัวอย่างในหน้าเว็บเท่านั้น โดยไม่ส่งข้อมูลไปเซิร์ฟเวอร์
  
  // บันทึกข้อความลง Supabase จริง
  messageForm.addEventListener("submit", async (event) => {
    event.preventDefault();
    clearError();

    const message = messageInput.value.trim();
    const selectedMode = document.querySelector(
      'input[name="mode"]:checked'
    )?.value;

    if (!message) {
      showError("กรุณาพิมพ์ข้อความก่อน");
      messageInput.focus();
      return;
    }

    if (message.length > 1000) {
      showError("ข้อความต้องไม่เกิน 1000 ตัวอักษร");
      messageInput.focus();
      return;
    }

    if (
      selectedMode === "named" &&
      !senderInput.value.trim()
    ) {
      showError("กรุณากรอกชื่อที่ต้องการแสดง หรือเลือกไม่เปิดเผยชื่อ");
      senderInput.focus();
      return;
    }

    const sender = selectedMode === "named"
      ? senderInput.value.trim()
      : "ไม่เปิดเผยชื่อ";

    const submitButton = messageForm.querySelector(
      'button[type="submit"], input[type="submit"]'
    );

    if (submitButton) submitButton.disabled = true;

    try {
      const response = await fetch(
        `${SUPABASE_URL}/rest/v1/veil_messages`,
        {
          method: "POST",
          
headers: {
  "Content-Type": "application/json",
  "apikey": SUPABASE_PUBLISHABLE_KEY,
  "Prefer": "return=minimal"
},

          },
          body: JSON.stringify({
            message: message,
            sender: sender
          })
        }
      );

      if (!response.ok) {
        const errorDetails = await response.text();
        console.error("Supabase insert error:", response.status, errorDetails);
        throw new Error(
          response.status === 401 || response.status === 403
            ? "Supabase ไม่อนุญาตให้บันทึกข้อความ กรุณาตรวจสอบสิทธิ์ตาราง"
            : "บันทึกข้อความไม่สำเร็จ กรุณาลองใหม่"
        );
      }

      previewMessage.textContent = message;
      previewSender.textContent = sender;
      showPage("result");

    } catch (error) {
      console.error("ส่งข้อความไม่สำเร็จ:", error);
      showError(
        error.message === "Failed to fetch"
          ? "เชื่อมต่อ Supabase ไม่สำเร็จ กรุณาตรวจสอบการเชื่อมต่อ"
          : error.message || "เกิดข้อผิดพลาด กรุณาลองใหม่"
      );
    } finally {
      if (submitButton) submitButton.disabled = false;
    }
  });


  // เขียนข้อความถึงคนเดิมอีกครั้ง
  againButton.addEventListener("click", () => {
    resetComposer(visitorName);
    showPage("compose");
    messageInput.focus({ preventScroll: true });
  });

  // เริ่มต้นใหม่และเปลี่ยนผู้รับ
  startOverButton.addEventListener("click", () => {
    visitorName = "";
    resetComposer();
    visitorNameInput.value = "";
    clearUsernameError();
    showPage("home");
    visitorNameInput.focus({ preventScroll: true });
  });
});
