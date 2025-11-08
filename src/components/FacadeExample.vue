<template>
  <div style="margin-bottom: 24px;">
    <h3>Facade 패턴 예제</h3>
    <button @click="bookTicket">티켓 예매하기</button>
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

// 세부 기능들을 각각 클래스로 분리
class SeatSelector {
  select(seat: string | string[]) {
    const seatText = Array.isArray(seat) ? seat.join(", ") : seat;
    console.log(`💺 좌석 ${seatText} 선택 완료`);
  }
}

class PriceCalculator {
  calculate(basePrice: number, audience: AudienceCount, snack?: string): number {
    const adult = audience.adult * basePrice;
    const teen = audience.teen * 12000;
    const senior = audience.senior * 10000;
    const disabled = audience.disabled * 8000;
    let total = adult + teen + senior + disabled;

    if (snack) {
      total += 11000;
      console.log(`🍿 스낵(${snack}) 추가: +11,000원`);
    }

    console.log(
        `👥 인원: 일반 ${audience.adult}, 청소년 ${audience.teen}, 우대 ${audience.senior}, 장애인 ${audience.disabled}`
    );
    console.log(`💵 총 결제 금액: ${total.toLocaleString()}원`);
    return total;
  }
}

class ReservationConfirmer {
  confirm(ticket: MovieTicket, totalPrice: number) {
    const seatText = Array.isArray(ticket.seat) ? ticket.seat.join(", ") : ticket.seat;
    console.log(
        `✅ 예매 확정: [${ticket.theater}] ${ticket.movie} ${ticket.time} / 좌석 ${seatText} / ${totalPrice.toLocaleString()}원`
    );
  }
}

// 💡 Facade: 복잡한 절차를 단일 메서드로 단순화
class ReservationFacade {
  private seatSelector = new SeatSelector();
  private priceCalculator = new PriceCalculator();
  private confirmer = new ReservationConfirmer();

  reserve(ticket: MovieTicket) {
    console.log("[Facade] 예매 절차 시작");
    this.seatSelector.select(ticket.seat);
    const total = this.priceCalculator.calculate(ticket.basePrice, ticket.audience, ticket.snack);
    this.confirmer.confirm(ticket, total);
    console.log("[Facade] 예매 완료!");
  }
}

export default {
  name: "FacadeExample",
  methods: {
    bookTicket() {
      console.clear();
      console.log("[Facade Pattern] 실행 시작");

      // ✅ Builder와 동일한 데이터 사용
      const ticket = new MovieTicket(
          "CGV 전주고사",
          "체인소맨: 레제편",
          "2025-11-08 19:30",
          ["G8", "G9"],
          { adult: 2, teen: 0, senior: 0, disabled: 0 },
          15000,
          "나쵸 콤보"
      );

      const facade = new ReservationFacade();
      facade.reserve(ticket);
    }
  }
};
</script>
