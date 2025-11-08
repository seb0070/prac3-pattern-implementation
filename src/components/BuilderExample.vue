<template>
  <div style="margin-bottom: 24px;">
    <h3>Builder 패턴 예제</h3>
    <button @click="createTicket">티켓 생성</button>
  </div>
</template>

<script lang="ts">
interface AudienceCount {
  adult: number;
  teen: number;
  senior: number;
  disabled: number;
}

class MovieTicket {
  constructor(
      public theater: string,
      public movie: string,
      public time: string,
      public seat: string | string[],
      public audience: AudienceCount,
      public basePrice: number,
      public snack?: string
  ) {}
}

class MovieTicketBuilder {
  private theater?: string;
  private movie?: string;
  private time?: string;
  private seat: string | string[] = "";
  private audience: AudienceCount = { adult: 0, teen: 0, senior: 0, disabled: 0 };
  private basePrice?: number;
  private snack?: string;

  setTheater(theater: string) { this.theater = theater; return this; }
  setMovie(movie: string) { this.movie = movie; return this; }
  setTime(time: string) { this.time = time; return this; }
  setSeat(seat: string | string[]) { this.seat = seat; return this; }
  setAudience(a: AudienceCount) { this.audience = { ...a }; return this; }
  setBasePrice(price: number) { this.basePrice = price; return this; }
  addSnack(snack: string) { this.snack = snack; return this; }

  // 데모용: 검증 없이 바로 생성 (입력 폼이 없으므로 단순화)
  build(): Readonly<MovieTicket> {
    console.log("🎟️ [Builder] 티켓 생성 완료");
    return Object.freeze(new MovieTicket(
        this.theater ?? "",
        this.movie ?? "",
        this.time ?? "",
        this.seat,
        { ...this.audience },
        this.basePrice ?? 15000, // 기본가 미설정 시 15,000으로
        this.snack
    ));
  }
}

export default {
  name: "BuilderExample",
  methods: {
    createTicket() {
      console.clear();
      console.log("[Builder Pattern] 실행 시작");

      const ticket = new MovieTicketBuilder()
          .setTheater("CGV 전주고사")
          .setMovie("체인소맨: 레제편")
          .setTime("2025-11-08 19:30")
          .setSeat(["G8", "G9"])
          .setAudience({ adult: 2, teen: 0, senior: 0, disabled: 0 })
          .setBasePrice(15000)
          .addSnack("나쵸 콤보")
          .build();

      console.log(ticket);
    }
  }
};
</script>
